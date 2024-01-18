## Low Issues

### [L-1] `Storage::addRentals()` lacks input validation for existing orders

`Storage::addRentals()` does not check whether there's an existing order with the same hash. While this probably won't be called outside the context of `Create::_rentFromZone()`, I believe it's still a good idea to have this check to prevent authorized addresses from manipulating it:

```solidity
function addRentals(
    bytes32 orderHash,
    RentalAssetUpdate[] memory rentalAssetUpdates
) external onlyByProxy permissioned {
    // Add the order to storage.
    orders[orderHash] = true;

    // Add the rented items to storage.
    for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
        RentalAssetUpdate memory asset = rentalAssetUpdates[i];

        // Update the order hash for that item.
        rentedAssets[asset.rentalId] += asset.amount;
    }
}
```

It should be made to revert whenever the order is found in storage instead of directly proceeding with updating the `rentedAssets` array.

### [L-2] Anyone can stop `BASE` rental orders through `Stop::stopRent()` and `Stop::stopRentBatch()`

`Stop::stopRent()` and `Stop::stopRentBatch()` do not do input validation for the order. Anyone can pass any order and as long as the hash of that order exists in `Storage`, it will let them proceed with the call. The only `msg.sender` validation for this function happens inside `_validateRentalCanBeStopped()`:

```solidity
function _validateRentalCanBeStoped(
      OrderType orderType,
      uint256 endTimestamp,
      address expectedLender
  ) internal view {
      // Determine if the order has expired.
      bool hasExpired = endTimestamp <= block.timestamp;

      // Determine if the fulfiller is the lender of the order.
      bool isLender = expectedLender == msg.sender;

      // BASE orders processing.
      if (orderType.isBaseOrder()) {
          // check that the period for the rental order has expired.
          if (!hasExpired) {
              revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
          }
      }
      // PAY order processing.
      else if (orderType.isPayOrder()) {
          // If the stopper is the lender, then it doesnt matter whether the rental
          // has expired. But if the stopper is not the lender, then the rental must have expired.
          if (!isLender && (!hasExpired)) {
              revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
          }
      }
      // Revert if given an invalid order type.
      else {
          revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
      }
}
```

Here, the code does perform checks if `expectedLender == msg.sender` in the case of `PAY` orders, but in the case of `BASE` orders it doesn't. This leads to anyone being able to cancel `BASE` orders provided that the rental period is over. I personally believe it's better to let only the renters and lenders stop their rental orders, not anyone. To mitigate this, a check should be added to verify that `msg.sender` is either the lender or the renter, otherwise it should revert.

### [L-3] Upgradeable ERC721 and ERC1155s can bypass `Guard` and withdraw tokens from rental safe

The docs provided state that dishonest ERC721 or ERC1155s implementations that include selectors that are not caught by `Guard::_checkTransaction()` will not be allowed into orders. Hence, it is a known issue. There is a caveat to that though because there could come orders that include upgradeable ERC721 or ERC1155s. In that case, at the time of the admin reviewing the order, there might not be any function selectors that allow evasion of the guard, but they could be introduced later on through an upgrade to the contract.

The exploit would work in the following way:

1. An order is created that includes an upgradeable ERC721 or ERC1155
2. An admin fulfills this order
3. Once the rental has been initiated, the token is moved into a safe wallet deployed by the protocol.
4. The token contract owner upgrades the token to include a function selector not caught by `Guard::_checkTransaction()`.
5. Then, the renter can abuse the change by calling the new function using `rentalSafe.executeTransaction()`, or the owner of the upgradeable contract can abuse it themselves. `Guard::_checkTransaction()` will be run but it won't revert because it is not aware of this new selector.
6. The token has been successfully stolen and the rental order is left in a broken state.

To prevent this, admins should not allow order creations with upgradeable tokens as well.

### [L-4] Rental order hashes are not unique across different chains

`Signer::getRentalOrderHash()` creates order hashes that are not chain agnostic. If two same orders exist on different chains, a signature replay can be performed through `Stop::stopRent()` or `Stop::stopRentBatch()`. The likelihood of two same orders existing on two distinct chains is small, and the impact of this is very minor. Another potential impact I could think of this is for the backends/frontends parsing the data to malfunction when two same order hashes are encountered, that would happen if they expect the order hashes to be unique in their logic. I recommend adhering to the EIP-4337 standard, which suggests the usage of `block.chainid` as a preventive measure against cross-chain signature replay attacks. It should be incorporated into `Signer::_deriveRentalOrderHash` in the following way:

```solidity
function _deriveRentalOrderHash(
    RentalOrder memory order
) internal view returns (bytes32) {
   // ...
    return
        keccak256(
            abi.encode(
                _RENTAL_ORDER_TYPEHASH,
                order.seaportOrderHash,
                keccak256(abi.encodePacked(itemHashes)),
                keccak256(abi.encodePacked(hookHashes)),
                order.orderType,
                order.lender,
                order.renter,
                order.startTimestamp,
                order.endTimestamp,
                block.chainid
            )
        );
}
```

### [L-5] `Admin::toggleWhitelistDelegate()` and `Admin::toggleWhitelistExtension()` are sandwichable

`Admin::toggleWhitelistDelegate()` and `Admin::toggleWhitelistExtension()` are vulnerable to sandwich attacks. These are only callable by authorized members and are expected to be trusted.

There are two scenarios where this could go wrong:

1. An admin accidentally adds a wrong address for `whitelistedDelegates` or `whitelistedExtensions`
2. There are upgradeable delegates or extensions that have been compromised and upgraded with a malicious implementation

If any of those scenarios were to occur, lenders and renters could take advantage of that and perform `DELEGATECALL` or `CALL` calls on the safes that allows them to access addresses that were previously forbidden by the `Guard` policy. The most destructive impact of this would probably be for them to perform a `delegatecall` to `Stop::reclaimRentalOrder()` and steal the funds. The mitigation would be to implement a pausing mechanism that allows safe interactions to be paused. The flow would be the following:

1. An admin finds out that a delegate or extension has been compromised
2. The admin pauses all rental safes
3. The admin then investigates further and removes the malicious extensions and delegates
4. Once everything is fixed, the admin re-enables the safes.

This could be implemented at the `Guard` policy's level.