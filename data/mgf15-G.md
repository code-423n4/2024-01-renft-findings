## G-1 Possible Optimizations in `Storage.sol`

> Summary:

* Replaced `for` loops with `do while` loops for better gas efficiency.
* Utilized assembly for more efficient arithmetic operations.
* Refactored the [`addRentals`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L189C5-L203C6), [`removeRentals`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L216C5-L235C6) functions for gas optimization.
* Updated the [`updateHookPath`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L294C5-L303C6) and [`updateHookStatus`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L313C5-L326C6) functions to include assembly optimizations.

> Gas Savings:

* addRentals: Gas savings of 32 (51497 → 51465)
* removeRentals: Gas savings of 33 (4619 → 4586)
* updateHookPath: Gas savings of 23 (33894 → 33871)
* updateHookStatus: Gas savings of 10 (26766 → 26756)
* removeRentalsBatch: Gas savings of 575 (6634 → 5059)

```diff
diff --git a/Storage.sol b/aStorage.sol
index 1e46bcb..6384d94 100644
--- a/Storage.sol
+++ b/aStorage.sol
@@ -194,12 +194,17 @@ contract Storage is Proxiable, Module, StorageBase {
         orders[orderHash] = true;
 
         // Add the rented items to storage.
-        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
+        // @audit use do while
+        uint256 i;
+        do{//for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];
 
             // Update the order hash for that item.
             rentedAssets[asset.rentalId] += asset.amount;
-        }
+            unchecked {
+                ++i;
+            }
+        }while(i < rentalAssetUpdates.length);
     }
 
     /**
@@ -226,12 +231,17 @@ contract Storage is Proxiable, Module, StorageBase {
         }
 
         // Process each rental asset.
-        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
+        // @audit use do while
+        uint256 i;
+        do{//for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];
 
             // Reduce the amount of tokens for the particular rental ID.
             rentedAssets[asset.rentalId] -= asset.amount;
-        }
+            unchecked {
+                ++i;
+            }
+        }while (i < rentalAssetUpdates.length);
     }
 
     /**
@@ -246,7 +256,9 @@ contract Storage is Proxiable, Module, StorageBase {
         RentalAssetUpdate[] calldata rentalAssetUpdates
     ) external onlyByProxy permissioned {
         // Delete the orders from storage.
-        for (uint256 i = 0; i < orderHashes.length; ++i) {
+        // @audit use do while
+        uint256 i;
+        do{//for (uint256 i = 0; i < orderHashes.length; ++i) {
             // The order must exist to be deleted.
             if (!orders[orderHashes[i]]) {
                 revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
@@ -254,15 +266,23 @@ contract Storage is Proxiable, Module, StorageBase {
                 // Delete the order from storage.
                 delete orders[orderHashes[i]];
             }
-        }
+            unchecked {
+                ++i;
+            }
+        }while(i < orderHashes.length);
 
         // Process each rental asset.
-        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
+        // @audit use do while
+        uint256 j;
+        do{//for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];
 
             // Reduce the amount of tokens for the particular rental ID.
             rentedAssets[asset.rentalId] -= asset.amount;
-        }
+            unchecked {
+                ++j;
+            }
+        }while(j < rentalAssetUpdates.length);
     }
 
     /**
@@ -291,12 +311,19 @@ contract Storage is Proxiable, Module, StorageBase {
      * @param to   Target address which will use a hook as middleware.
      * @param hook Address of the hook which will act as a middleware.
      */
+    //@audit assembly method available 
     function updateHookPath(address to, address hook) external onlyByProxy permissioned {
         // Require that the `to` address is a contract.
-        if (to.code.length == 0) revert Errors.StorageModule_NotContract(to);
+        uint256 _codeto;
+        uint256 _codehook;
+        assembly{
+            _codeto := extcodesize(to)
+            _codehook := extcodesize(hook)
+        }
+        if (_codeto == 0) revert Errors.StorageModule_NotContract(to);
 
         // Require that the `hook` address is a contract.
-        if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);
+        if (_codehook == 0) revert Errors.StorageModule_NotContract(hook);
 
         // Point the `to` address to the `hook` address.
         _contractToHook[to] = hook;
@@ -310,12 +337,17 @@ contract Storage is Proxiable, Module, StorageBase {
      * @param hook   Address of the hook contract.
      * @param bitmap Decimal value that defines the active functionality on the hook.
      */
+    //@audit assembly method available 
     function updateHookStatus(
         address hook,
         uint8 bitmap
     ) external onlyByProxy permissioned {
         // Require that the `hook` address is a contract.
-        if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);
+        uint256 _codehook;
+        assembly{
+            _codehook := extcodesize(hook)
+        }
+        if (_codehook == 0) revert Errors.StorageModule_NotContract(hook);
 
         // 7 is 0x00000111. This ensures that only a valid bitmap can be set.
         if (bitmap > uint8(7))

```

> Gas diff

```diff
diff --git a/1.txt b/2.txt
index 39f808b..40931a6 100644
--- a/1.txt
+++ b/2.txt
@@ -1,5 +1,5 @@
-| addRentals                               | 1046            | 49008 | 51497  | 53997 | 40      |
-| removeRentals                            | 680             | 4857  | 4619   | 11246 | 11      |
-| updateHookPath                           | 565             | 28439 | 33894  | 36394 | 28      |
-| updateHookStatus                         | 546             | 24091 | 26766  | 33766 | 31      |
-| removeRentalsBatch                       | 805             | 6248  | 6634   | 11526 | 6       |
+| addRentals                               | 1046            | 48978 | 51465  | 53965 | 40      |
+| removeRentals                            | 680             | 4833  | 4586   | 11246 | 11      |
+| updateHookPath                           | 565             | 28511 | 33871  | 36371 | 28      |
+| updateHookStatus                         | 546             | 24081 | 26756  | 33756 | 31      |
+| removeRentalsBatch                       | 805             | 5403  | 5059   | 11501 | 6       |
```

## G-2 Possible Optimizations in `PaymentEscrow.sol` 

> Summary:

* Utilized assembly for more efficient arithmetic operations in the [`_calculateFee`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88C5-L91C6) and [`_calculatePaymentProRata`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L132C5-L147C1) functions.
* Refactored the [`_decreaseDeposit`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L292C4-L295C6), and [`_increaseDeposit`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L304C5-L307C6) functions for gas optimization.
* Updated the [`_settlePaymentBatch`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L215C5-L283C6) function to include a `do while` loop for better gas efficiency.

> Gas Savings:

* `increaseDeposit`: Gas savings of 72 (28641 → 28569)
* `settlePayment`: Gas savings of 84 (14903 → 14819)
* `settlePaymentBatch`: Gas savings of 291 (28910 → 28619)
    
```diff
diff --git a/PaymentEscrow.sol b/aPaymentEscrow.sol
index 34c4f4c..3763c7d 100644
--- a/PaymentEscrow.sol
+++ b/aPaymentEscrow.sol
@@ -85,9 +85,15 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
      *
      * @param amount Amount for which to calculate the fee.
      */
+    //@audit use assembly for math
     function _calculateFee(uint256 amount) internal view returns (uint256) {
         // Uses 10,000 as a denominator for the fee.
-        return (amount * fee) / 10000;
+        uint256 ret;
+        assembly{
+            ret := mul(amount,fee.slot)
+            ret := div(ret,10000)
+        }
+        return ret;//(amount * fee) / 10000;
     }
 
     /**
@@ -129,20 +135,29 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
      * @return renterAmount Payment amount to send to the renter.
      * @return lenderAmount Payment amoutn to send to the lender.
      */
+    //@audit use assembly for math
     function _calculatePaymentProRata(
         uint256 amount,
         uint256 elapsedTime,
         uint256 totalTime
     ) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {
         // Calculate the numerator and adjust by a multiple of 1000.
-        uint256 numerator = (amount * elapsedTime) * 1000;
+        uint256 numerator ;//= (amount * elapsedTime) * 1000;
 
         // Calculate the result, but bump by 500 to add a rounding adjustment. Then,
         // reduce by a multiple of 1000.
-        renterAmount = ((numerator / totalTime) + 500) / 1000;
+        renterAmount ;//= ((numerator / totalTime) + 500) / 1000;
 
         // Calculate lender amount from renter amount so no tokens are left behind.
-        lenderAmount = amount - renterAmount;
+        lenderAmount ;//= amount - renterAmount;
+        assembly{
+            numerator := mul(amount,elapsedTime)
+            numerator := mul(numerator,1000)
+            renterAmount := div(numerator,totalTime)
+            renterAmount := add(renterAmount,500)
+            renterAmount := div(renterAmount,1000)
+            lenderAmount := sub(amount,renterAmount)
+        }
     }
 
     /**
@@ -228,7 +243,9 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
         bool isRentalOver = elapsedTime >= totalTime;
 
         // Loop through each item in the order.
-        for (uint256 i = 0; i < items.length; ++i) {
+        //@audit use do while
+        uint256 i;
+        do{//for (uint256 i = 0; i < items.length; ++i) {
             // Get the item.
             Item memory item = items[i];
 
@@ -279,7 +296,10 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                     revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
                 }
             }
-        }
+            unchecked {
+                ++i;
+            }
+        }while(i<items.length);
     }
 
     /**
@@ -289,9 +309,18 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
      * @param token  Token address.
      * @param amount Amount to decrease the balance by.
      */
+    //@audit Possible Optimizations in `_decreaseDeposit`
     function _decreaseDeposit(address token, uint256 amount) internal {
         // Directly decrease the synced balance.
-        balanceOf[token] -= amount;
+        //balanceOf[token] -= amount;
+        uint256 _amount = balanceOf[token];
+        assembly {
+            let amt := sub(_amount,amount)
+            mstore(0, token)
+            mstore(32, balanceOf.slot)
+            let ref := keccak256(0, 64)
+            sstore(ref, amt)
+     }
     }
 
     /**
@@ -301,9 +330,18 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
      * @param token  Token address.
      * @param amount Amount to increase the balance by.
      */
+    //@audit Possible Optimizations in `_increaseDeposit`
     function _increaseDeposit(address token, uint256 amount) internal {
         // Directly increase the synced balance.
-        balanceOf[token] += amount;
+        //balanceOf[token] += amount;
+        uint256 _amount = balanceOf[token];
+        assembly {
+            let amt := add(_amount,amount)
+            mstore(0, token)
+            mstore(32, balanceOf.slot)
+            let ref := keccak256(0, 64)
+            sstore(ref, amt)
+     }
     }
 
     /////////////////////////////////////////////////////////////////////////////////
@@ -338,7 +376,9 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
         RentalOrder[] calldata orders
     ) external onlyByProxy permissioned {
         // Loop through each order.
-        for (uint256 i = 0; i < orders.length; ++i) {
+        //@audit use do while
+        uint256 i;
+        do{//for (uint256 i = 0; i < orders.length; ++i) {
             // Settle all payments for the order.
             _settlePayment(
                 orders[i].items,
@@ -348,7 +388,10 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 orders[i].startTimestamp,
                 orders[i].endTimestamp
             );
-        }
+            unchecked {
+                ++i;
+            }
+        }while(i < orders.length);
     }
 
     /**
@@ -431,3 +474,4 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
         _freeze();
     }
 }
+

```

> Gas diff

```diff
diff --git a/1.txt b/2.txt
index cca92e0..cb0c636 100644
--- a/1.txt
+++ b/2.txt
@@ -1,3 +1,3 @@
-| increaseDeposit                                      | 2741            | 22728 | 28641  | 31141 | 45      |
-| settlePayment                                        | 4248            | 13849 | 14903  | 21928 | 14      |
-| settlePaymentBatch                                   | 4439            | 23017 | 28910  | 29812 | 4       |
+| increaseDeposit                                      | 2669            | 22659 | 28569  | 31069 | 45      |
+| settlePayment                                        | 4248            | 13692 | 14819  | 21462 | 14      |
+| settlePaymentBatch                                   | 4439            | 22795 | 28619  | 29505 | 4       |
```


## G-3 Possible Optimizations in `Stop.sol::stopRentBatch`

> Summary:

* Replaced `for` loops with `do while` loops for better gas efficiency in the stopRentBatch function.
* Refactored the [`stopRentBatch`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265C5-L306C6) function for gas optimization.

> Gas Savings:

* stopRentBatch: Gas savings of 3250 (191879 → 188629)

```diff
diff --git a/Stop.sol b/aStop.sol
index ab240ba..24fc0a2 100644
--- a/Stop.sol
+++ b/aStop.sol
@@ -321,7 +321,8 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
 
         // Process each rental order.
         // Memory will become safe after this block.
-        for (uint256 i = 0; i < orders.length; ++i) {
+        uint256 i;
+        do{//for (uint256 i = 0; i < orders.length; ++i) {
             // Check that the rental can be stopped.
             _validateRentalCanBeStoped(
                 orders[i].orderType,
@@ -330,7 +331,8 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
             );
 
             // Check if each item in the order is a rental. If so, then generate the rental asset update.
-            for (uint256 j = 0; j < orders[i].items.length; ++j) {
+            uint256 j;
+            do{//for (uint256 j = 0; j < orders[i].items.length; ++j) {
                 // Insert the rental asset update into the dynamic array.
                 if (orders[i].items[j].isRental()) {
                     _insert(
@@ -339,7 +341,10 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
                         orders[i].items[j].amount
                     );
                 }
-            }
+                unchecked {
+                    ++j;
+                }
+            }while(j < orders[i].items.length);
 
             // Add the order hash to an array.
             orderHashes[i] = _deriveRentalOrderHash(orders[i]);
@@ -354,7 +359,10 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
 
             // Emit rental order stopped.
             _emitRentalOrderStopped(orderHashes[i], msg.sender);
-        }
+            unchecked {
+                ++i;
+            }
+        }while(i < orders.length);

```

> Gas diff

```diff
+| stopRentBatch                       | 172872          | 191879 | 191879 | 210887 | 2       |
-| stopRentBatch                       | 169622          | 188629 | 188629 | 207637 | 2       |
```
