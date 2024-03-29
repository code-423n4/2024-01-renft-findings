# Findings Summary

| ID            | Title                                                                | Severity |
| ------------- | -------------------------------------------------------------------- | -------- |
| [L-01](#l-01) | Lack of minimum signers & minimum threshold check                    | Low      |
| [L-02](#l-02) | `Create.validateOrder` function : `zoneParams` can be replayed       | Low      |
| [L-03](#l-03) | `Create.validateOrder` function : renter `signature` can be replayed | Low      |
| [L-04](#l-04) | `PaymentEscrow` can be bricked by some payment tokens                                                                                         | Low      |

# Low

## [L-01] Lack of minimum signers & minimum threshold check <a id="l-01" ></a>

## Details

- In the current `Factory.deployRentalSafe` function design: for risk mitigation, it is suggested to set a minimum of **2 for the `threshold`** and a **minimum of 3 signers/owners**.

- By doing this; two different signers votes will be needed before a transaction is executed; so in case of a one signer account is compromised; the other two signers will be able to mitigate his actions/change signers, also; if a signer is down, the other two signers will be able to execute transactions out of the SAFE wallet.

## Proof of Concept

[Factory.deployRentalSafe function](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L138)

```javascript
function deployRentalSafe(
```

## Tools Used

Manual Testing.

## Recommendation

Ensure that the minimum owners number is >= 3, and minimum threshold is >= 2.

## [L-02] `Create.validateOrder` function : `zoneParams` can be replayed <a id="l-02" ></a>

## Details

`validateOrder` function is invoked by the SeaPort with the `zoneParams` for the order to be fulfilled, where this argument holds the rental payload and the renter signature, but there's a possibility of replaying the same order again even if it has an expiry for the signature.

## Proof of Concept

[Create.validateOrder function](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L733-L735)

```javascript
  function validateOrder(
        ZoneParameters calldata zoneParams
    ) external override onlyRole("SEAPORT") returns (bytes4 validOrderMagicValue) {
```

## Tools Used

Manual Testing.

## Recommendation

Save the executed `zoneParams` in a mapping, and check against it whenever `validateOrder` is invoked.

## [L-03] `Create.validateOrder` function : renter `signature` can be replayed <a id="l-03" ></a>

## Details

When `validateOrder` function is invoked by the SeaPort, the order fulfiller signature is recovered and validated,but there's a possibility of replaying the same siganture again even if it has an expiry for the signature.

## Proof of Concept

[Create.validateOrder function](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L759-L763)

```javascript
   // Recover the signer from the payload.
        address signer = _recoverSignerFromPayload(
            _deriveRentPayloadHash(payload),
            signature
        );
```

[Signer.\_recoverSignerFromPayload function](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L107-L116)

```javascript
   function _recoverSignerFromPayload(
        bytes32 payloadHash,
        bytes memory signature
    ) internal view returns (address) {
        // Derive original EIP-712 digest using domain separator and order hash.
        bytes32 digest = _DOMAIN_SEPARATOR.toTypedDataHash(payloadHash);

        // Recover the signer address of the signature.
        return digest.recover(signature);
    }
```

## Tools Used

Manual Testing.

## Recommendation

Save the recovered signature in a mapping, and check against it whenever `validateOrder` is invoked.
## [L-04] `PaymentEscrow` can be bricked by some payment tokens<a id="l-04" ></a>

## Impact

- Some tokens as [`cUSDCv3`](https://etherscan.io/address/0xbfc4feec175996c08c8f3a0469793a7979526065#code) (Compound USDC) has a special case when the user determines the amount to be transferred to be equals to `type(uint256).max` while his balance is less than this amount; in this case his balance will be transferred instead of reverting the transaction:

  ```javascript
  if (amount == type(uint256).max) {
    amount = balanceOf(src);
  }
  ```

- How could this brick the `PaymentEscrow` with `cUSDCv3` token as the payment token and resulting in locking the rented assets?

  1.  Assume that there's a `BASE` rental order with `cUSDCv3` token as it's payment token.
  2.  A malicious renter has a dust amount of this token (1 wei for example), then he fulfills this rental order by paying the protocol `type(uint256).max`, so the escrow will record this max amount as being deposited in it.
  3.  Now the rental time is over, but if anyone tries to stop the rental by calling `Stop.stopRentals`; the transaction will revert as the balance of the escrow is way less than it has actually received (the escrow contract received the dust amount instead of receiving the `type(uint256).max`, and the recorded ` balanceOf[token]` is set to `type(uint256).max`).

- This will result in permanently locking the rented assets in the renter SAFE wallet.

## Proof of Concept

[Create.\_rentFromZone function/L599-L603](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L599-L603)

```javascript
    for (uint256 i = 0; i < items.length; ++i) {
                if (items[i].isERC20()) {
                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
                }
            }
```

[PaymentEscrow.\_increaseDeposit function](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L304-L307)

```javascript
    function _increaseDeposit(address token, uint256 amount) internal {
        // Directly increase the synced balance.
        balanceOf[token] += amount;
    }
```

## Tools Used

Manual Review.

## Recommended Mitigation Steps

Prevent using such tokens in the protocol.
