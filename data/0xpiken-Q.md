## [L&#x2011;01] Calculate `rentalOrderTypeHash` with wrong encoding function
It is incorrect to use `abi.encode()` for type hash calculation of `rentalOrderTypeHash` in [`Singer#_deriveRentalTypehashes()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L339-L408):
```solidity
373:        rentalOrderTypeHash = keccak256(
374:            abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)
375:        );
```
To mitigate this, use `abi.encodePacked()` instead:
```diff
        rentalOrderTypeHash = keccak256(
-           abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)
+           abi.encodePacked(rentalOrderTypeString, hookTypeString, itemTypeString)
        );
```