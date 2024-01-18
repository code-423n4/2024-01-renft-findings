# Gas Efficiency Report
## Summary:

The code review has identified three key issues in the smart contract repository. Firstly, in [G-01], it is recommended to optimize gas usage by calling `ensureValidRole` only if the `isRole` function returns false. Secondly, in [G-02], the subtraction operation in PaymentEscrow.sol #145 is suggested to be surrounded by `unchecked` as it is considered safe. Lastly, in [G-03], adding an else statement between two if statements in Reclaimer.sol #98 is advised to avoid unnecessary checks.

## Summary Table:

| **Issue Number** | **Title**                                | **Number of Instances** | **Instance Location**                                                        |
|------------------|------------------------------------------|--------------------------|-------------------------------------------------------------------------------|
| G-01             | Optimize `ensureValidRole` invocation    | 1                        | [Kernel.sol #316](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L316)   |
| G-02             | Use `unchecked` for Safe Subtraction     | 1                        | [PaymentEscrow.sol #145](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L145) |
| G-03             | Add `else` statement for efficiency     | 1                        | [Reclaimer.sol #98](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L98) |
## [G-01] `ensureValidRolerole` should be called only if the role is new
Add an if statement to call `ensureValidRole` only if the isRole returns false saving gas performing unnecessary checks 
### Instances
*[Kernel.sol #316](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L316)
```solidity
        ensureValidRole(role_);

```



## [G-02] Surround by unchecked 
The following subtraction is safe to surround by unchecked
```solidity
        lenderAmount = amount - renterAmount;

```
it can be interpreted as `amount - (amount x (elapsedTime / totalTime))` which is safe as` amount` is always >= `amount x (elapsedTime / totalTime)`
### Instances
* [PaymentEscrow.sol #145](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L145)

## [G-03] Add else statement to avoid unnecessary checks
### Instances
* [Reclaimer.sol #98](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L98)
```solidity
            if (item.itemType == ItemType.ERC721)
                _transferERC721(item, rentalOrder.lender);

            // check if the item is an ERC1155.
+           //add an else statement between the two if statements 
            if (item.itemType == ItemType.ERC1155)
                _transferERC1155(item, rentalOrder.lender);
        }
```