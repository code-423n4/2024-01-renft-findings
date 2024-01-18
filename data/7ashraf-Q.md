# Quality Assurance Report
## Summary

The smart contract repository review identified key issues, primarily the need for equal-length array checks in functions like `_processBaseOrderOffer` and others. Instances of using array objects without validation were noted in `_checkTransaction` and sections of `Create.sol`, suggesting the implementation of validation checks. Additionally, the `executeAction` function in `Kernel.sol` lacks a final `else` statement with a revert, impacting event accuracy. Hardcoded zero values, as seen in `Stop.sol`, could benefit from more descriptive values. Lastly, functions with empty bodies and unclear readability indicate areas for code clarity improvement.

**Summary Table:**

| **Issue Number** | **Title** | **Number of Instances** |
|------------------|-----------|-------------------------|
| L-01 | Require the two arrays to have the same length | 6 |
| L-02 | Check if the array object is valid or not | 7 |
| L-03 | Add final `else` statement with revert to avoid false emits | 1 |
| L-04 | Add a list of known roles to choose from them instead of validating roles | 1 |
| L-05 | Avoid using hardcoded zero value | 1 |
| L-06 | Disable fee changing when a settle payment is running | 1 |
| L-07 | `skim` should be called after `settlePayment` and `settlePaymentBatch` | 1 |
| N-01 | Hard to read function, consider cleaning | 1 |
| N-02 | Empty function body | 1 |
| N-03 | Emit an event on fee change | 2 |
| N-04 | Reverting with an error is more convenient than returning address(0) | 1 |
| N-05 | Reduce redundant code | 1 |
## [L-01] Require the two arrays to have the same length
### Instances
* [Create.sol #195](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L195)
```solidity
function _processBaseOrderOffer(
        Item[] memory rentalItems,
        SpentItem[] memory offers,
        uint256 startIndex
    ) internal pure {
+        require rentalItems.length == offers.length;
        ...
    }
```
* [Create.sol #247](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L247)
```solidity
function _processPayOrderOffer(
        Item[] memory rentalItems,
        SpentItem[] memory offers,
        uint256 startIndex
    ) internal pure {
+        require rentalItems.length == offers.length;
        ...
    }
```
* [Create.sol #326](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L326)
```solidity
    function _processBaseOrderConsideration(
        Item[] memory rentalItems,
        ReceivedItem[] memory considerations,
        uint256 startIndex
    ) internal pure {
+        require rentalItems.length == considerations.length;
        ...
    }
```

* [Create.sol #411](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L411)
```solidity
    function _convertToItems(
        SpentItem[] memory offers,
        ReceivedItem[] memory considerations,
        OrderType orderType
    ) internal pure returns (Item[] memory items) {
+        require offers.length == considerations.length;
        ...
    }
```
* [Create.sol #464](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L464)
```solidity
    function _addHooks(
        Hook[] memory hooks,
        SpentItem[] memory offerItems,
        address rentalWallet
    ) internal {
+        require hooks.length == offerItems.length;
        ...
    }
```
* [Stop.sol #194](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L194)
```solidity
    function _removeHooks(
        Hook[] calldata hooks,
        Item[] calldata rentalItems,
        address rentalWallet
    ) internal {
+        require hooks.length == rentalItems.length;
        ...
    }
```

## [L-02] Check if the array object is valid or not 
### Instances

* [Create.sol #377](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L377)
```solidity
            ReceivedItem memory consideration = considerations[i];
+           //check for valid consideration
```
* [Create.sol #211](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L211)
```solidity
            SpentItem memory offer = offers[i];

```
* [Create.sol #263](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L263)
```solidity
            SpentItem memory offer = offers[i];

```
* [Create.sol #339](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L339)
```solidity
            ReceivedItem memory consideration = considerations[i];

```
* [Create.sol #477](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L477)
```solidity
            target = hooks[i].target;

```
* [Storage.sol #141](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L141)

```solidity
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
```
* [Storage.sol #230](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L230)
```solidity
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
```
## [L-03] Add final `else` statement with revert to avoid false emits
### Instances
* [Kernel.sol #299](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L299)
```solidity
    function executeAction(Actions action_, address target_) external onlyExecutor {
        if (action_ == Actions.InstallModule) {
            ensureContract(target_);
            ensureValidKeycode(Module(target_).KEYCODE());
            _installModule(Module(target_));
        } else if (action_ == Actions.UpgradeModule) {
            ensureContract(target_);
            ensureValidKeycode(Module(target_).KEYCODE());
            _upgradeModule(Module(target_));
        } else if (action_ == Actions.ActivatePolicy) {
            ensureContract(target_);
            _activatePolicy(Policy(target_));
        } else if (action_ == Actions.DeactivatePolicy) {
            ensureContract(target_);
            _deactivatePolicy(Policy(target_));
        } else if (action_ == Actions.MigrateKernel) {
            ensureContract(target_);
            _migrateKernel(Kernel(target_));
        } else if (action_ == Actions.ChangeExecutor) {
            executor = target_;
        } else if (action_ == Actions.ChangeAdmin) {
            admin = target_;
        }
+       //add else statement and revert
        emit Events.ActionExecuted(action_, target_);
    }
```

## [L-04] Add a list of known roles to choose from them instead of validating roles
### Instances
* [Kernel.sol #316](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L316)
```solidity
    function grantRole(Role role_, address addr_) public onlyAdmin {
        ...
        emit Events.RoleGranted(role_, addr_);
    }
```

## [L-05] Avoid using hardcoded zero value
### Instances
* [Stop.sol #172](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L172)
```solidity
        bool success = ISafe(order.rentalWallet).execTransactionFromModule(
            // Stop policy inherits the reclaimer package.
            address(this),
            // value.
172:        0,
            // The encoded call to the `reclaimRentalOrder` function.
            abi.encodeWithSelector(this.reclaimRentalOrder.selector, order),
            // Safe must delegate call to the stop policy so that it is the msg.sender.
            Enum.Operation.DelegateCall
        );
```


## [L-06] Disable fee changing when a settle payment is running
### Instances
* [PaymentEscrow.sol #380](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L380)
```solidity
    function setFee(uint256 feeNumerator) external onlyByProxy permissioned {
+       //Add lock
        // Cannot accept a fee numerator greater than 10000.
        if (feeNumerator > 10000) {
            revert Errors.PaymentEscrow_InvalidFeeNumerator();
        }

        // Set the fee.
        fee = feeNumerator;
+       //Release lock
    }
```
## [L-07] `skim` should be called after `settlePayment` and `settlePaymentBatch`
### Instances
* [PaymentEscrow.sol #320](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L320)
```solidity
    function settlePayment(RentalOrder calldata order) external onlyByProxy permissioned {
        // Settle all payments for the order.
        _settlePayment(
            order.items,
            order.orderType,
            order.lender,
            order.renter,
            order.startTimestamp,
            order.endTimestamp
        );
    }
```


## [N-01] Hard to read function, consider cleaning
### Instances
* [Guard.sol #195](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195)
```solidity
function _checkTransaction(address from, address to, bytes memory data) private view {
    ...
    }
```
### Mitigation
* Consider adding more comments
* splitting the code into multiple methods

## [N-02] Empty function body
Although it is mentioned that the function is left empty, but is not implementedbefore an audit or a final release.
Also no reason was mentioned.
### Instances
* [Guard.sol #353](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L353)
```solidity
    /**
     * @notice Performs any checks after execution. This is left unimplemented.
     *
     * @param txHash Hash of the transaction.
     * @param success Whether the transaction succeeded.
     */
    function checkAfterExecution(bytes32 txHash, bool success) external override {}

    /**
```


## [N-03] Emit an event on fee change 
### Instances
* [PaymentEscrow.sol #380](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L380)
```solidity
    function setFee(uint256 feeNumerator) external onlyByProxy permissioned {
        // Cannot accept a fee numerator greater than 10000.
        if (feeNumerator > 10000) {
            revert Errors.PaymentEscrow_InvalidFeeNumerator();
        }

        // Set the fee.
        fee = feeNumerator;
+       //emit fee chanaged event
    }
```




* [PaymentEscrow.sol #337](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L337)
```solidity

    function settlePaymentBatch(
        RentalOrder[] calldata orders
    ) external onlyByProxy permissioned {
        // Loop through each order.
        for (uint256 i = 0; i < orders.length; ++i) {
            // Settle all payments for the order.
            _settlePayment(
                orders[i].items,
                orders[i].orderType,
                orders[i].lender,
                orders[i].renter,
                orders[i].startTimestamp,
                orders[i].endTimestamp
            );
        }
    }
```


## [N-04] Reverting with an error is more convenient than returning address(0)
Consider adding a revert message like `failed to fetch` or `hook not available` instead of returning address(0) to the user for more convenient and understandable logic
### Instances
* [Storage.sol #141](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L141)
```solidity
        return hookStatus[hook] != 0 ? hook : address(0);
```

## [N-05] Reduce redundant code
Utilize the use of removeRental inside removeRentalBatch instead of repeating the same logic all over again
### Instances
* [Storage.sol #244](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L244)
### Mitigation
Rewrite the following method 
```solidity
    function removeRentalsBatch(
        bytes32[] calldata orderHashes,
        RentalAssetUpdate[] calldata rentalAssetUpdates
    ) external onlyByProxy permissioned {
        // Delete the orders from storage.
        for (uint256 i = 0; i < orderHashes.length; ++i) {
            // The order must exist to be deleted.
            if (!orders[orderHashes[i]]) {
                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
            } else {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            }
        }

        // Process each rental asset.
        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }
    }
```
into something like this 
```solidity
    function removeRentalsBatch(
        bytes32[] calldata orderHashes,
        RentalAssetUpdate[] calldata rentalAssetUpdates
    ) external onlyByProxy permissioned {
        // Delete the orders from storage.
        for (uint256 i = 0; i < orderHashes.length; ++i) {
           removeRental(orderHash[i], rentalAssetUpdates);
        }

       
    }
```