## [L-01] Lack of Bytecode validation in `PaymentEscrow::_safeTransfer` function

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L102 

The `PaymentEscrow::_safeTransfer` function does not validate whether the `token` address provided contains bytecode or not. 
This lack of validation poses a potential risk, as the function assumes that the address is always a valid ERC20 token contract.

```javascript
        // Call transfer() on the token.
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(IERC20.transfer.selector, to, value)
        );

```

Consider adding pre-condition check in the `PaymentEscrow::_safeTransfer` function to verify that the  `token` address contains a bytecode. 

## [L-02] Potential Revert in `PaymentEscrow._settlePaymentProRata` due to zero token value transfer

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L168C6-L168C6

The `PaymentEscrow::_settlePaymentProRata` function is designed to handle pro-rata payment settlements for `PAY` lend orders. 

However, there is a potential edge case leading to unintended behavior. When a PAY lend order is terminated just before its scheduled end time, the internal function `_calculatePaymentProRata` may round a payment of 0.5 tokens in favour of the renter. 

This rounding can result in the calculated token amount for the lender being zero. 
Consequently, attempting a token transfer of zero value can cause the function to revert, especially if the token contract does not allow zero-value transfers.

```javascript
        // Calculate the pro-rata payment for renter and lender.
        (uint256 renterAmount, uint256 lenderAmount) = _calculatePaymentProRata(
            amount,
            elapsedTime,
            totalTime
        );

```

To address this issue, it is recommended to add a conditional check within the `_settlePaymentProRata` function to handle the case where the calculated token amount for the lender is zero. 

## [L-03] NFT Transfer Prior to State Update in `Stop::stopRent` Function

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L293C5-L302C11

In the `Stop::stopRent` function, there is an order of operations issue where the NFT transfer is executed before updating the protocol state. 
The `_reclaimRentedItems(order)` function, which handles the NFT transfer, is called prior to the `STORE.removeRentals` function that updates the protocol state.

This ordering breaks the widely used checks-effects-interactions pattern for smart contract development. 

In `src/policies/Stop.sol`
```javascript
293       _reclaimRentedItems(order);
294
295        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
296        ESCRW.settlePayment(order);
297
298        // Interaction: Remove rentals from storage by computing the order hash.
299        STORE.removeRentals(
300            _deriveRentalOrderHash(order),
301            _convertToStatic(rentalAssetUpdates)
302        );
```

To address this issue, it is recommended to reorder the operations so that the protocol state is updated before executing the NFT transfer. 

```diff
-        _reclaimRentedItems(order);

-        ESCRW.settlePayment(order);

        STORE.removeRentals(
            _deriveRentalOrderHash(order),
            _convertToStatic(rentalAssetUpdates)
        );

+        _reclaimRentedItems(order); // NFT Transfer

+        ESCRW.settlePayment(order);

```


## [I-01] Unused import of `OrderFulfillment` in `Create.sol`

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L25C13-L25C13

In the `Create.sol` contract, there is an import statement for `OrderFulfillment` from the `RentalStructs.sol` library. However, it is not utilized in any part of the contract's code. 

It is advisable to remove the import statement for `OrderFulfillment` from the `Create.sol` contract. This action will enhance the gas optimization during deployment. 

In `src/policies/Create.sol` file:
```diff
import {
    RentalOrder,
    RentPayload,
    SeaportPayload,
    Hook,
-   OrderFulfillment,
    OrderMetadata,
    OrderType,
    Item,
    ItemType,
    SettleTo,
    RentalId,
    RentalAssetUpdate
} from "@src/libraries/RentalStructs.sol";
```
