# reNFT GAS OPTIMIZATIONS


## INTRODUCTION
Highlighted below are optimizations exclusively targeting state-mutating functions and view/pure functions invoked by state-mutating functions. In the discussion that follows, only runtime gas is emphasized, given its inevitable dominance over deployment gas costs throughout the protocol's lifetime. 

Please be aware that some code snippets may be shortened to conserve space, and certain code snippets may include @audit tags in comments to facilitate issue explanations."




## [G-01] Avoid Reading and writing to state if amount is zero

### 4 Instances

1. #### Refactor `PaymentEscrow._decreaseDeposit()` to avoid state read/write if `amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L294

In the `PaymentEscrow._decreaseDeposit()` function as shown below checks should be implemented to avoid  reading and writing to state if the `amount` argument is zero this is because if `amount` is 0 the statement `balanceOf[token] -= amount` would not change the value of `spenderAllowance` since its being decremented by zero. This then means that in scenarios where `amount` is 0 the statement `balanceOf[token] -= amount` is re-assigning the same value to state i.e there is no state change. Also its important to note that the `_settlePayment()` function that invokes this function does note implement this check before the invocation. The diff below shows how the function should be refactored:

```solidity
file: src/modules/PaymentEscrow.sol

292:    function _decreaseDeposit(address token, uint256 amount) internal {
293:        // Directly decrease the synced balance.
294:        balanceOf[token] -= amount;
295:    }
```
```diff
diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
index 34c4f4c..d90f184 100644
--- a/src/modules/PaymentEscrow.sol
+++ b/src/modules/PaymentEscrow.sol
@@ -291,7 +291,10 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
      */
     function _decreaseDeposit(address token, uint256 amount) internal {
         // Directly decrease the synced balance.
-        balanceOf[token] -= amount;
+        if (amount > 0){
+            balanceOf[token] -= amount;
+        }
+
     }
```
```
Estimated gas saved: 5000 gas units
```


2. #### Refactor `Storage.addRentals()` to avoid state read/write if `asset.amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L201

In the `Storage.addRentals()` function as shown below checks should be implemented to avoid reading and writing to state if the value of  `asset.amount` is zero this is because if `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] += asset.amount` would not change the value of `rentedAssets[asset.rentalId]` since its being incremented by zero. This then means that in scenarios where `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] += asset.amount` is re-assigning the same value to state i.e there is no state change. The diff below shows how the function should be refactored:

```solidity
file: src/modules/Storage.sol

189:    function addRentals(
190:        bytes32 orderHash,
191:        RentalAssetUpdate[] memory rentalAssetUpdates
192:    ) external onlyByProxy permissioned {
193:        // Add the order to storage.
194:        orders[orderHash] = true;
195:
196:        // Add the rented items to storage.
197:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
198:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
199:
200:            // Update the order hash for that item.
201:            rentedAssets[asset.rentalId] += asset.amount;
202:        }
203:    }
```
```diff
diff --git a/src/modules/Storage.sol b/src/modules/Storage.sol
index 1e46bcb..b909f41 100644
--- a/src/modules/Storage.sol
+++ b/src/modules/Storage.sol
@@ -198,7 +198,10 @@ contract Storage is Proxiable, Module, StorageBase {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];

             // Update the order hash for that item.
-            rentedAssets[asset.rentalId] += asset.amount;
+            if (asset.amount > 0) {
+                rentedAssets[asset.rentalId] += asset.amount;
+            }
+
         }
     }
```
```
Estimated gas saved: 5000 gas units
```

3. #### Refactor `Storage.removeRentals()` to avoid state read/write if `asset.amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L233

In the `Storage.removeRentals()` function as shown below checks should be implemented to avoid reading and writing to state if the value of  `asset.amount` is zero this is because if `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` would not change the value of `rentedAssets[asset.rentalId]` since its being decremented by zero. This then means that in scenarios where `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` is re-assigning the same value to state i.e there is no state change. The diff below shows how the function should be refactored:

```solidity
file: src/modules/Storage.sol

216:    function removeRentals(
217:        bytes32 orderHash,
218:        RentalAssetUpdate[] calldata rentalAssetUpdates
219:    ) external onlyByProxy permissioned {
220:        // The order must exist to be deleted.
221:        if (!orders[orderHash]) {
222:            revert Errors.StorageModule_OrderDoesNotExist(orderHash);
223:        } else {
224:            // Delete the order from storage.
225:            delete orders[orderHash];
226:        }
227:
228:        // Process each rental asset.
229:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
230:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
231:
232:            // Reduce the amount of tokens for the particular rental ID.
233:            rentedAssets[asset.rentalId] -= asset.amount;
234:        }
235:    }
```
```diff
diff --git a/src/modules/Storage.sol b/src/modules/Storage.sol
index 1e46bcb..b8f2248 100644
--- a/src/modules/Storage.sol
+++ b/src/modules/Storage.sol
@@ -230,7 +230,10 @@ contract Storage is Proxiable, Module, StorageBase {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];

             // Reduce the amount of tokens for the particular rental ID.
-            rentedAssets[asset.rentalId] -= asset.amount;
+            if (asset.amount > 0) {
+                rentedAssets[asset.rentalId] -= asset.amount;
+            }
+
         }
     }
```
```
Estimated gas saved: 5000 gas units
```

4. #### Refactor `Storage.removeRentalsBatch()` to avoid state read/write if `asset.amount` is zero
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L264

In the `Storage.removeRentalsBatch()` function as shown below checks should be implemented to avoid reading and writing to state if the value of  `asset.amount` is zero this is because if `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` would not change the value of `rentedAssets[asset.rentalId]` since its being decremented by zero. This then means that in scenarios where `asset.amount` is 0 the statement `rentedAssets[asset.rentalId] -= asset.amount` is re-assigning the same value to state i.e there is no state change. The diff below shows how the function should be refactored:

```solidity
file: src/modules/Storage.sol

244:    function removeRentalsBatch(
245:        bytes32[] calldata orderHashes,
246:        RentalAssetUpdate[] calldata rentalAssetUpdates
247:    ) external onlyByProxy permissioned {
248:        // Delete the orders from storage.
249:        for (uint256 i = 0; i < orderHashes.length; ++i) {
250:            // The order must exist to be deleted.
251:            if (!orders[orderHashes[i]]) {
252:                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
253:            } else {
254:                // Delete the order from storage.
255:                delete orders[orderHashes[i]];
256:            }
257:        }
258:
259:        // Process each rental asset.
260:        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
261:            RentalAssetUpdate memory asset = rentalAssetUpdates[i];
262:
263:            // Reduce the amount of tokens for the particular rental ID.
264:            rentedAssets[asset.rentalId] -= asset.amount;
265:        }
266:    }
```
```diff
diff --git a/src/modules/Storage.sol b/src/modules/Storage.sol
index 1e46bcb..677656c 100644
--- a/src/modules/Storage.sol
+++ b/src/modules/Storage.sol
@@ -261,7 +261,10 @@ contract Storage is Proxiable, Module, StorageBase {
             RentalAssetUpdate memory asset = rentalAssetUpdates[i];

             // Reduce the amount of tokens for the particular rental ID.
-            rentedAssets[asset.rentalId] -= asset.amount;
+            if (asset.amount > 0) {
+                rentedAssets[asset.rentalId] -= asset.amount;
+            }
+
         }
```
```
Estimated gas saved: 5000 gas units
```



## [G-02]  Pre-calculate equations which contain only constant values in constructor
For equations, calcuations and computations that only involve constants or immutable values they could be calculated in the contract's constructor and saved to an immutable variable. Using the immutable variable would be cheaper than performing the calculation every time the function is called.

### 3 Instances

1. #### Save `toKeycode("STORE")` computation to an Immutable variable
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L48-#L49
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L73
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L77
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L80-#L81


The free function `toKeycode()` converts a `bytes5` value into a `Keycode` type and returns the `Keycode` value. Calling `toKeycode()` on a constant `bytes5` value would always return the same `Keycode` value therefore calling `toKeycode("STORE")` would always result in the same value. So  having to call `toKeycode("STORE")` multiple times in the `configureDependencies()` and `requestPermissions()` functions isn't gas efficient rather we should make the call in the constructor then save the returned value to an immutable variable then the immutable variable be used in place of `toKeycode("STORE")` in functions that invokes it. The diff below shows howthe code should be refactored:

```solidity
file: src/policies/Admin.sol

40:    function configureDependencies()
41:        external
42:        override
43:        onlyKernel
44:        returns (Keycode[] memory dependencies)
45:    {
46:        dependencies = new Keycode[](2);
47:
48:        dependencies[0] = toKeycode("STORE");  //@audit toKeycode("STORE")
49:        STORE = Storage(getModuleAddress(toKeycode("STORE")));  //@audit toKeycode("STORE")
50:
51:        dependencies[1] = toKeycode("ESCRW");
52:        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
53:    }
.
.
.
64:    function requestPermissions()
65:        external
66:        view
67:        override
68:        onlyKernel
69:        returns (Permissions[] memory requests)
70:    {
71:        requests = new Permissions[](8);
72:        requests[0] = Permissions(
73:            toKeycode("STORE"),  //@audit toKeycode("STORE")
74:            STORE.toggleWhitelistExtension.selector
75:        );
76:        requests[1] = Permissions(
77:            toKeycode("STORE"),  //@audit toKeycode("STORE")
78:            STORE.toggleWhitelistDelegate.selector
79:        );
80:        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);  //@audit toKeycode("STORE")
81:        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);  //@audit toKeycode("STORE")
82:
83:        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
84:        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
85:        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
86:        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
87:    }
```
```diff
diff --git a/src/policies/Admin.sol b/src/policies/Admin.sol                                
index b37d47f..d19a99c 100644                                                               
--- a/src/policies/Admin.sol                                                                
+++ b/src/policies/Admin.sol                                                                
@@ -20,13 +20,16 @@ contract Admin is Policy {                                              
     // Modules that the policy depends on.                                                 
     Storage public STORE;                                                                  
     PaymentEscrow public ESCRW;                                                            
+    Keycode immutable storeKeycode;                                                        
                                                                                            
     /**                                                                                    
      * @dev Instantiate this contract as a policy.                                         
      *                                                                                     
      * @param kernel_ Address of the kernel contract.                                      
      */                                                                                    
-    constructor(Kernel kernel_) Policy(kernel_) {}                                         
+    constructor(Kernel kernel_) Policy(kernel_) {                                          
+        storeKeycode = toKeycode("STORE");                                                 
+    }                                                                                      
                                                                                            
     /**                                                                                    
      * @notice Upon policy activation, configures the modules that the policy depends on.  
@@ -45,8 +48,8 @@ contract Admin is Policy {                                                
     {                                                                                      
         dependencies = new Keycode[](2);                                                   
                                                                                            
-        dependencies[0] = toKeycode("STORE");                                              
-        STORE = Storage(getModuleAddress(toKeycode("STORE")));                             
+        dependencies[0] = storeKeycode;                                                    
+        STORE = Storage(getModuleAddress(storeKeycode));                                   
                                                                                            
         dependencies[1] = toKeycode("ESCRW");                                              
         ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));                       
@@ -70,15 +73,15 @@ contract Admin is Policy {                                              
     {                                                                                      
         requests = new Permissions[](8);                                                   
         requests[0] = Permissions(                                                         
-            toKeycode("STORE"),                                                            
+            storeKeycode,                                                                  
             STORE.toggleWhitelistExtension.selector                                        
         );                                                                                 
         requests[1] = Permissions(                                                         
-            toKeycode("STORE"),                                                            
+            storeKeycode,
             STORE.toggleWhitelistDelegate.selector
         );
-        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
-        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);
+        requests[2] = Permissions(storeKeycode, STORE.upgrade.selector);
+        requests[3] = Permissions(storeKeycode, STORE.freeze.selector);

         requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
         requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
```

#### Apply the same changes to these Instances in the `Create`, `Factory`, `Guard` and `Stop` Contracts.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L80-#L81
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L104
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L81-#L82
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L102
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L71-#L72
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L92-#L93
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L71-#L72
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L95-#L96



2. #### Save `toKeycode("ESCRW")` computation to an Immutable variable
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L51-#L52
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L83-#L86

The free function `toKeycode()` converts a `bytes5` value into a `Keycode` type and returns the `Keycode` value. Calling `toKeycode()` on a constant `bytes5` value would always return the same `Keycode` value therefore calling `toKeycode("ESCRW")` would always result in the same value. So  having to call `toKeycode("ESCRW")` multiple times in the `configureDependencies()` and `requestPermissions()` functions isn't gas efficient rather we should make the call in the constructor then save the returned value to an immutable variable then the immutable variable be used in place of `toKeycode("ESCRW")` in functions that invokes it. The diff below shows howthe code should be refactored:

```solidity
file: src/policies/Admin.sol

40:    function configureDependencies()
41:        external
42:        override
43:        onlyKernel
44:        returns (Keycode[] memory dependencies)
45:    {
46:        dependencies = new Keycode[](2);
47:
48:        dependencies[0] = toKeycode("STORE");  
49:        STORE = Storage(getModuleAddress(toKeycode("STORE")));
50:
51:        dependencies[1] = toKeycode("ESCRW");    //@audit toKeycode("ESCRW")
52:        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW"))); //@audit toKeycode("ESCRW")
53:    }
.
.
.
64:    function requestPermissions()
65:        external
66:        view
67:        override
68:        onlyKernel
69:        returns (Permissions[] memory requests)
70:    {
71:        requests = new Permissions[](8);
72:        requests[0] = Permissions(
73:            toKeycode("STORE"),
74:            STORE.toggleWhitelistExtension.selector
75:        );
76:        requests[1] = Permissions(
77:            toKeycode("STORE"),
78:            STORE.toggleWhitelistDelegate.selector
79:        );
80:        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
81:        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);
82:
83:        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);  //@audit toKeycode("ESCRW")
84:        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);  //@audit toKeycode("ESCRW")
85:        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);  //@audit toKeycode("ESCRW")
86:        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);  //@audit toKeycode("ESCRW")
87:    }
```
```diff
diff --git a/src/policies/Admin.sol b/src/policies/Admin.sol
index b37d47f..d2ae37b 100644
--- a/src/policies/Admin.sol
+++ b/src/policies/Admin.sol
@@ -20,13 +20,15 @@ contract Admin is Policy {
     // Modules that the policy depends on.
     Storage public STORE;
     PaymentEscrow public ESCRW;
-
+    Keycode immutable escrwKeycode;
     /**
      * @dev Instantiate this contract as a policy.
      *
      * @param kernel_ Address of the kernel contract.
      */
-    constructor(Kernel kernel_) Policy(kernel_) {}
+    constructor(Kernel kernel_) Policy(kernel_) {
+        escrwKeycode = toKeycode("ESCRW");
+    }

     /**
      * @notice Upon policy activation, configures the modules that the policy depends on.
@@ -48,8 +50,8 @@ contract Admin is Policy {
         dependencies[0] = toKeycode("STORE");
         STORE = Storage(getModuleAddress(toKeycode("STORE")));

-        dependencies[1] = toKeycode("ESCRW");
-        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
+        dependencies[1] = escrwKeycode;
+        ESCRW = PaymentEscrow(getModuleAddress(escrwKeycode));
     }

     /**
@@ -80,10 +82,10 @@ contract Admin is Policy {
         requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
         requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);

-        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
-        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
-        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
-        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
+        requests[4] = Permissions(escrwKeycode, ESCRW.skim.selector);
+        requests[5] = Permissions(escrwKeycode, ESCRW.setFee.selector);
+        requests[6] = Permissions(escrwKeycode, ESCRW.upgrade.selector);
+        requests[7] = Permissions(escrwKeycode, ESCRW.freeze.selector);
     }
```

#### Apply the same changes to these Instances in the `Create` and `Stop` Contracts.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L83-#L84
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L105
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L74-#L75
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L97-#L98




## [G-03] Move lesser gas costing require/revert checks to the top
Require() or Revert() statements that check input arguments or cost lesser gas should be at the top of the function.
Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting alot of gas in a function that may ultimately revert in the unhappy case.

### 2 Instances
1. #### Move `if (!ISafe(safe).isOwner(owner)) {revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)` to the top of the function.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L647-#L657

In the `_isValidSafeOwner()` function as shown below the if-revert statement `if (STORE.deployedSafes(safe) == 0) {revert Errors.CreatePolicy_InvalidRentalSafe(safe);}` is more gas consumimg than the if-revert statement `if (!ISafe(safe).isOwner(owner)) {revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)` as the former reads from state which cost `2100` gas units. We can make the `_isValidSafeOwner()` function more gas efficient by moving the cheaper if-revert statement `if (!ISafe(safe).isOwner(owner)) {revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)` to the top of the function so that in scenarios where the cheaper revert statement fails the function would revert without having to read from state which is expensive. The diff below shows how the code could be refactored:

```solidity
file: src/policies/Create.sol

647:    function _isValidSafeOwner(address owner, address safe) internal view {
648:        // Make sure only protocol-deployed safes can rent.
649:        if (STORE.deployedSafes(safe) == 0) {
650:            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
651:        }
652:
653:        // Make sure the fulfiller is the owner of the recipient rental safe.
654:        if (!ISafe(safe).isOwner(owner)) {  //@audit move check to function top
655:            revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);
656:        }
657:    }
```

```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..d0eb13a 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -645,15 +645,17 @@ contract Create is Policy, Signer, Zone, Accumulator {
      * @param safe  Address of the potential protocol-deployed rental safe.
      */
     function _isValidSafeOwner(address owner, address safe) internal view {
-        // Make sure only protocol-deployed safes can rent.
-        if (STORE.deployedSafes(safe) == 0) {
-            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
-        }

         // Make sure the fulfiller is the owner of the recipient rental safe.
         if (!ISafe(safe).isOwner(owner)) {
             revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);
         }
+
+        // Make sure only protocol-deployed safes can rent.
+        if (STORE.deployedSafes(safe) == 0) {
+            revert Errors.CreatePolicy_InvalidRentalSafe(safe);
+        }
+
     }
```
```
Estimated gas saved: 2100
```

2. #### Move `if (data.length < 4) {revert Errors.GuardPolicy_FunctionSelectorRequired()}` to the top of the function.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L324-#L331

In the `checkTransaction()` function as shown below the if-revert statement `if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {revert Errors.GuardPolicy_UnauthorizedDelegateCall(to)}` is more gas consumimg than the if-revert statement `if (data.length < 4) {revert Errors.GuardPolicy_FunctionSelectorRequired()}` as the former reads from state and makes an external call which would cost at least `2200` gas units. We can make the 
`checkTransaction()` function more gas efficient by moving the cheaper if-revert statement `if (data.length < 4) {revert Errors.GuardPolicy_FunctionSelectorRequired()}` to the top of the function so that in scenarios where the cheaper revert statement fails the function would revert without having to read from state and make an external call which would be  expensive. The diff below shows how the code could be refactored:

```solidity
file: src/policies/Guard.sol

309:    function checkTransaction(
310:        address to,
311:        uint256 value,
312:        bytes memory data,
313:        Enum.Operation operation,
314:        uint256,
315:        uint256,
316:        uint256,
317:        address,
318:        address payable,
319:        bytes memory,
320:        address
321:    ) external override {
322:        // Disallow transactions that use delegate call, unless explicitly
323:        // permitted by the protocol.
324:        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
325:            revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
326:        }
327:
328:        // Require that a function selector exists.
329:        if (data.length < 4) {   //@audit move check to function top
330:            revert Errors.GuardPolicy_FunctionSelectorRequired();
331:        }
.
.
.
345:    }
```
```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index a0a565a..b8d25f9 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -319,16 +319,19 @@ contract Guard is Policy, BaseGuard {
         bytes memory,
         address
     ) external override {
+
+        // Require that a function selector exists.
+        if (data.length < 4) {
+            revert Errors.GuardPolicy_FunctionSelectorRequired();
+        }
+
         // Disallow transactions that use delegate call, unless explicitly
         // permitted by the protocol.
         if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
             revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
         }

-        // Require that a function selector exists.
-        if (data.length < 4) {
-            revert Errors.GuardPolicy_FunctionSelectorRequired();
-        }
+

         // Fetch the hook to interact with for this transaction.
         address hook = STORE.contractToHook(to);
```
```
Estimated gas saved: 2200 gas units
```



## [G-04] Cache state variables outside of loop to avoid reading storage on every iteration
Reading from storage should always try to be avoided within loops. In the following instances, we are able to cache state variables outside of the loop to save a Gwarmaccess (100 gas) per loop iteration.

#### Please note these instances were not included in the bots reports

### 5 Instances
1. #### Cache `fee` outside the loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L242

In the `_settlePayment()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `fee` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

```solidity
file: src/modules/PaymentEscrow.sol

215:    function _settlePayment(
216:        Item[] calldata items,
217:        OrderType orderType,
218:        address lender,
219:        address renter,
220:        uint256 start,
221:        uint256 end
222:    ) internal {
223:        // Calculate the time values.
224:        uint256 elapsedTime = block.timestamp - start;
225:        uint256 totalTime = end - start;
226:
227:        // Determine whether the rental order has ended.
228:        bool isRentalOver = elapsedTime >= totalTime;
229:
230:        // Loop through each item in the order.
231:        for (uint256 i = 0; i < items.length; ++i) {
232:            // Get the item.
233:            Item memory item = items[i];
234:
235:            // Check that the item is a payment.
236:            if (item.isERC20()) {
237:                // Set a placeholder payment amount which can be reduced in the
238:                // presence of a fee.
239:                uint256 paymentAmount = item.amount;
240:
241:                // Take a fee on the payment amount if the fee is on.
242:                if (fee != 0) { //@audit cache fee outside the loop
243:                    // Calculate the new fee.
244:                    uint256 paymentFee = _calculateFee(paymentAmount);
245:
246:                    // Adjust the payment amount by the fee.
247:                    paymentAmount -= paymentFee;
248:                }
.
.
.
283:    }
```
```diff
diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
index 34c4f4c..066d593 100644
--- a/src/modules/PaymentEscrow.sol
+++ b/src/modules/PaymentEscrow.sol
@@ -226,7 +226,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {

         // Determine whether the rental order has ended.
         bool isRentalOver = elapsedTime >= totalTime;
-
+        uint256 _fee = fee;
         // Loop through each item in the order.
         for (uint256 i = 0; i < items.length; ++i) {
             // Get the item.
@@ -239,7 +239,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 uint256 paymentAmount = item.amount;

                 // Take a fee on the payment amount if the fee is on.
-                if (fee != 0) {
+                if (_fee != 0) {
                     // Calculate the new fee.
                     uint256 paymentFee = _calculateFee(paymentAmount);
```
```
Estimated gas saved: 97 gas units per iteration
```

2. #### Cache `STORE` outside the loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L480

In the `_addHooks()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `STORE` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

```solidity
file: src/policies/Create.sol

464:    function _addHooks(
465:        Hook[] memory hooks,
466:        SpentItem[] memory offerItems,
467:        address rentalWallet
468:    ) internal {
469:        // Define hook target, offer item index, and an offer item.
470:        address target;
471:        uint256 itemIndex;
472:        SpentItem memory offer;
473:
474:        // Loop through each hook in the payload.
475:        for (uint256 i = 0; i < hooks.length; ++i) {
476:            // Get the hook's target address.
477:            target = hooks[i].target;
478:
479:            // Check that the hook is reNFT-approved to execute on rental start.
480:            if (!STORE.hookOnStart(target)) {   //@audit cache STORE outside the loop
481:                revert Errors.Shared_DisabledHook(target);
482:            }
483:
484:            // Get the offer item index for this hook.
485:            itemIndex = hooks[i].itemIndex;
.
.
.
520:    }
```
```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..a8de17a 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -470,14 +470,14 @@ contract Create is Policy, Signer, Zone, Accumulator {
         address target;
         uint256 itemIndex;
         SpentItem memory offer;
-
+        Storage _store = STORE;
         // Loop through each hook in the payload.
         for (uint256 i = 0; i < hooks.length; ++i) {
             // Get the hook's target address.
             target = hooks[i].target;

             // Check that the hook is reNFT-approved to execute on rental start.
-            if (!STORE.hookOnStart(target)) {
+            if (!_store.hookOnStart(target)) {
                 revert Errors.Shared_DisabledHook(target);
             }
```
```
Estimated gas saved: 97 gas units per iteration
```

3. #### Cache `ESCRW` outside of loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L601

In the `_rentFromZone()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `ESCRW` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

```solidity
file: src/policies/Create.sol

530:    function _rentFromZone(
531:        RentPayload memory payload,
532:        SeaportPayload memory seaportPayload
533:    ) internal {
534:        // Check: make sure order metadata is valid with the given seaport order zone hash.
535:        _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);
536:
537:        // Check: verify the fulfiller of the order is an owner of the recipient safe.
538:        _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);
.
.
.
597:            // Interaction: Increase the deposit value on the payment escrow so
598:            // it knows how many tokens were sent to it.
599:            for (uint256 i = 0; i < items.length; ++i) {
600:                if (items[i].isERC20()) {
601:                    ESCRW.increaseDeposit(items[i].token, items[i].amount); //@audit cache ESCRW outside loop
602:                }
603:            }
604:
605:            // Interaction: Process the hooks associated with this rental.
606:            if (payload.metadata.hooks.length > 0) {
607:                _addHooks(
608:                    payload.metadata.hooks,
609:                    seaportPayload.offer,
610:                    payload.fulfillment.recipient
611:                );
612:            }
613:
614:            // Emit rental order started.
615:            _emitRentalOrderStarted(order, orderHash, payload.metadata.emittedExtraData);
616:        }
617:    }
```
```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..1a8ecb6 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -593,12 +593,12 @@ contract Create is Policy, Signer, Zone, Accumulator {

             // Interaction: Update storage only if the order is a Base Order or Pay order.
             STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates));
-
+            PaymentEscrow _escrw = ESCRW;
             // Interaction: Increase the deposit value on the payment escrow so
             // it knows how many tokens were sent to it.
             for (uint256 i = 0; i < items.length; ++i) {
                 if (items[i].isERC20()) {
-                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
+                    _escrw.increaseDeposit(items[i].token, items[i].amount);
                 }
             }
```
```
Estimated gas saved: 97 gas units per iteration
```

4. #### Cache `ESCRW` outside of loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L700

In the `_executionInvariantChecks()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `ESCRW` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

```solidity
file: src/policies/Create.sol

691:    function _executionInvariantChecks(
692:        ReceivedItem[] memory executions,
693:        address expectedRentalSafe
694:    ) internal view {
695:        for (uint256 i = 0; i < executions.length; ++i) {
696:            ReceivedItem memory execution = executions[i];
697:
698:            // ERC20 invariant where the recipient must be the payment escrow.
699:            if (execution.isERC20()) {
700:                _checkExpectedRecipient(execution, address(ESCRW));
701:            }
702:            // ERC721 and ERC1155 invariants where the recipient must
703:            // be the expected rental safe.
704:            else if (execution.isRental()) {
705:                _checkExpectedRecipient(execution, expectedRentalSafe);
706:            }
707:            // Revert if unsupported item type.
708:            else {
709:                revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
710:                    execution.itemType
711:                );
712:            }
713:        }
714:    }
```
```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..93d1ec6 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -692,12 +692,13 @@ contract Create is Policy, Signer, Zone, Accumulator {
         ReceivedItem[] memory executions,
         address expectedRentalSafe
     ) internal view {
+        PaymentEscrow _escrw = ESCRW;
         for (uint256 i = 0; i < executions.length; ++i) {
             ReceivedItem memory execution = executions[i];

             // ERC20 invariant where the recipient must be the payment escrow.
             if (execution.isERC20()) {
-                _checkExpectedRecipient(execution, address(ESCRW));
+                _checkExpectedRecipient(execution, address(_escrw));
             }
             // ERC721 and ERC1155 invariants where the recipient must
             // be the expected rental safe.
```
```
Estimated gas saved: 97 gas saved per iteration
```

5. #### Cache `STORE` outside of loop
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L210

In the `_removeHooks()` function as shown below up to `100` gas units could be saved per iteration if we cache the state variable `STORE` in a stack variable and read from the stack variable rather than from state during the loop iterations. The function could be refactored as shown in the diff below:

```solidity
file: src/policies/Stop.sol

194:    function _removeHooks(
195:        Hook[] calldata hooks,
196:        Item[] calldata rentalItems,
197:        address rentalWallet
198:    ) internal {
199:        // Define hook target, item index, and item.
200:        address target;
201:        uint256 itemIndex;
202:        Item memory item;
203:
204:        // Loop through each hook in the payload.
205:        for (uint256 i = 0; i < hooks.length; ++i) {
206:            // Get the hook address.
207:            target = hooks[i].target;
208:
209:            // Check that the hook is reNFT-approved to execute on rental stop.
210:            if (!STORE.hookOnStop(target)) {
211:                revert Errors.Shared_DisabledHook(target);
212:            }
.
.
.
250:    }
```
```diff
diff --git a/src/policies/Stop.sol b/src/policies/Stop.sol
index ab240ba..c348c1c 100644
--- a/src/policies/Stop.sol
+++ b/src/policies/Stop.sol
@@ -200,14 +200,14 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
         address target;
         uint256 itemIndex;
         Item memory item;
-
+        Storage _store = STORE;
         // Loop through each hook in the payload.
         for (uint256 i = 0; i < hooks.length; ++i) {
             // Get the hook address.
             target = hooks[i].target;

             // Check that the hook is reNFT-approved to execute on rental stop.
-            if (!STORE.hookOnStop(target)) {
+            if (!_store.hookOnStop(target)) {
                 revert Errors.Shared_DisabledHook(target);
             }
```
```
Estimated gas saved: 97 gas saved per iteration
```


## [G-05] Cache external calls outside of loop to avoid re-calling function on each iteration
Performing STATICCALLs that do not depend on variables incremented in loops should always try to be avoided within the loop. In the instance, we are able to cache the external calls outside of the loop to save a STATICCALL (100 gas) per loop iteration.

### 1 Instance
1. #### Perform the external calls `orderType.isPayOrder()` and `orderType.isBaseOrder()` outside the loop and cache their results.
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L255

The external calls `orderType.isPayOrder()` and `orderType.isBaseOrder()` should be made outside the loop and the result cached since their returned values are not dependent on the loop iterations. The diff below shows how the code should be refactored:

```solidity
file: src/modules/PaymentEscrow.sol

215:    function _settlePayment(
216:        Item[] calldata items,
217:        OrderType orderType,
218:        address lender,
219:        address renter,
220:        uint256 start,
221:        uint256 end
222:    ) internal {
223:        // Calculate the time values.
224:        uint256 elapsedTime = block.timestamp - start;
225:        uint256 totalTime = end - start;
226:
227:        // Determine whether the rental order has ended.
228:        bool isRentalOver = elapsedTime >= totalTime;
229:
230:        // Loop through each item in the order.
231:        for (uint256 i = 0; i < items.length; ++i) {
232:            // Get the item.
233:            Item memory item = items[i];
234:
235:            // Check that the item is a payment.
236:            if (item.isERC20()) {
237:                // Set a placeholder payment amount which can be reduced in the
238:                // presence of a fee.
239:                uint256 paymentAmount = item.amount;
240:
241:                // Take a fee on the payment amount if the fee is on.
242:                if (fee != 0) {
243:                    // Calculate the new fee.
244:                    uint256 paymentFee = _calculateFee(paymentAmount);
245:
246:                    // Adjust the payment amount by the fee.
247:                    paymentAmount -= paymentFee;
248:                }
249:
250:                // Effect: Decrease the token balance. Use the payment amount pre-fee
251:                // so that fees can be taken.
252:                _decreaseDeposit(item.token, item.amount);
253:
254:                // If its a PAY order but the rental hasn't ended yet.
255:                if (orderType.isPayOrder() && !isRentalOver) {  //@audit make orderType.isPayOrder() outside loop
256:                    // Interaction: a PAY order which hasnt ended yet. Payout is pro-rata.
257:                    _settlePaymentProRata(
258:                        item.token,
259:                        paymentAmount,
260:                        lender,
261:                        renter,
262:                        elapsedTime,
263:                        totalTime
264:                    );
265:                }
266:                // If its a PAY order and the rental is over, or, if its a BASE order.
267:                else if (
268:                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()   //@audit make orderType.isPayOrder() and orderType.isBaseOrder() outside loop
269:                ) {
270:                    // Interaction: a pay order or base order which has ended. Payout is in full.
271:                    _settlePaymentInFull(
272:                        item.token,
273:                        paymentAmount,
274:                        item.settleTo,
275:                        lender,
276:                        renter
277:                    );
278:                } else {
279:                    revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
280:                }
281:            }
282:        }
283:    }
```
```diff
diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
index 34c4f4c..988b175 100644
--- a/src/modules/PaymentEscrow.sol
+++ b/src/modules/PaymentEscrow.sol
@@ -226,6 +226,8 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {

         // Determine whether the rental order has ended.
         bool isRentalOver = elapsedTime >= totalTime;
+        bool isPayOrder = orderType.isPayOrder();
+        bool isBaseOrder = orderType.isBaseOrder();

         // Loop through each item in the order.
         for (uint256 i = 0; i < items.length; ++i) {
@@ -252,7 +254,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 _decreaseDeposit(item.token, item.amount);

                 // If its a PAY order but the rental hasn't ended yet.
-                if (orderType.isPayOrder() && !isRentalOver) {
+                if (isPayOrder && !isRentalOver) {
                     // Interaction: a PAY order which hasnt ended yet. Payout is pro-rata.
                     _settlePaymentProRata(
                         item.token,
@@ -265,7 +267,7 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
                 }
                 // If its a PAY order and the rental is over, or, if its a BASE order.
                 else if (
-                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
+                    (isPayOrder && isRentalOver) || isBaseOrder
                 ) {
                     // Interaction: a pay order or base order which has ended. Payout is in full.
                     _settlePaymentInFull(
```
```
Estimated gas saved: 300 gas units
```



## [G-06] Refactor external/internal functions to avoid unnecessary SLOAD
The functions below read storage slots that are previously read in the functions that invoke them. We can refactor the external/internal functions to pass cached storage variables as stack variables and avoid the extra storage reads that would otherwise take place in the internal functions.

### 1 Instance
1. #### Refactor `stopRent()`, `stopRentBatch()` and `_removeHooks()` to avoid  `SLOAD` in `_removeHooks()`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L194-#L365

The external functions `stopRent()`, `stopRentBatch()` reads the variable `STORE` from state but they also invoke function `_removeHooks()` which also read the variable `STORE` from state. We can a avoid an `SLOAD(Gwarmaccess)` in the `_removeHooks()` function if we cache the `STORE` state variable in the `stopRent()` and `stopRentBatch()` functions then pass the cached value as a parameter to the `_removeHooks()` function: The diff below shows how the code should be refactored: 

```solidity
file: src/policies/Stop.sol

194:    function _removeHooks(
195:        Hook[] calldata hooks,
196:        Item[] calldata rentalItems,
197:        address rentalWallet
198:    ) internal {
199:        // Define hook target, item index, and item.
200:        address target;
201:        uint256 itemIndex;
202:        Item memory item;
203:
204:        // Loop through each hook in the payload.
205:        for (uint256 i = 0; i < hooks.length; ++i) {
206:            // Get the hook address.
207:            target = hooks[i].target;
208:
209:            // Check that the hook is reNFT-approved to execute on rental stop.
210:            if (!STORE.hookOnStop(target)) {     //@audit STORE state variable read
211:                revert Errors.Shared_DisabledHook(target);
212:            }
.
.
.
250:    }



265:    function stopRent(RentalOrder calldata order) external {
.
.
.
288:        if (order.hooks.length > 0) {
289:            _removeHooks(order.hooks, order.items, order.rentalWallet); //@audit STORE state variable read in _removeHooks function
290:        }
291:
292:        // Interaction: Transfer rentals from the renter back to lender.
293:        _reclaimRentedItems(order);
294:
295:        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
296:        ESCRW.settlePayment(order);
297:
298:        // Interaction: Remove rentals from storage by computing the order hash.
299:        STORE.removeRentals(     //@audit STORE state variable read
300:            _deriveRentalOrderHash(order),
301:            _convertToStatic(rentalAssetUpdates)
302:        );
303:
304:        // Emit rental order stopped.
305:        _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);
306:    }



313:    function stopRentBatch(RentalOrder[] calldata orders) external {
.
.
.
348:            if (orders[i].hooks.length > 0) {
349:                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet); //@audit STORE state variable read in _removeHooks function
350:            }
351:
352:            // Interaction: Transfer rental assets from the renter back to lender.
353:            _reclaimRentedItems(orders[i]);
354:
355:            // Emit rental order stopped.
356:            _emitRentalOrderStopped(orderHashes[i], msg.sender);
357:        }
358:
359:        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
360:        ESCRW.settlePaymentBatch(orders);
361:
362:        // Interaction: Remove all rentals from storage.
363:        STORE.removeRentalsBatch(orderHashes, _convertToStatic(rentalAssetUpdates)); //@audit STORE state variable read
364:    }
```
```diff
diff --git a/src/policies/Stop.sol b/src/policies/Stop.sol
index ab240ba..cbc0d12 100644
--- a/src/policies/Stop.sol
+++ b/src/policies/Stop.sol
@@ -194,7 +194,8 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
     function _removeHooks(
         Hook[] calldata hooks,
         Item[] calldata rentalItems,
-        address rentalWallet
+        address rentalWallet,
+        Storage _store
     ) internal {
         // Define hook target, item index, and item.
         address target;
@@ -283,10 +284,10 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
                 );
             }
         }
-
+        Storage _store = STORE;
         // Interaction: process hooks so they no longer exist for the renter.
         if (order.hooks.length > 0) {
-            _removeHooks(order.hooks, order.items, order.rentalWallet);
+            _removeHooks(order.hooks, order.items, order.rentalWallet, _store);
         }

         // Interaction: Transfer rentals from the renter back to lender.
@@ -296,7 +297,7 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
         ESCRW.settlePayment(order);

         // Interaction: Remove rentals from storage by computing the order hash.
-        STORE.removeRentals(
+        _store.removeRentals(
             _deriveRentalOrderHash(order),
             _convertToStatic(rentalAssetUpdates)
         );
@@ -343,10 +344,10 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {

             // Add the order hash to an array.
             orderHashes[i] = _deriveRentalOrderHash(orders[i]);
-
+            Storage _store = STORE;
             // Interaction: Process hooks so they no longer exist for the renter.
             if (orders[i].hooks.length > 0) {
-                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet);
+                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet, _store);
             }

             // Interaction: Transfer rental assets from the renter back to lender.
```



## [G-07] Multiple accesses of a array should use a local variable cache
The instances below point to the second+ access of a value inside an array, within a function. Caching an array's struct avoids re-calculating the array offsets into memory

### Proof of concept 
```solidity
struct Person {
    string name;
    uint age;
    uint id;
}

contract NoCacheArrayElement {

    Person[] students;

    function createStudents() external  {
        Person[] memory arrayOfPersons = new Person[](3); 
        Person memory newPerson1 = Person("Emmanuel", 15,1);
        Person memory newPerson2 = Person("Faustina", 16,2);
        Person memory newPerson3 = Person("Emmanuela", 14,3);

        arrayOfPersons[0] = newPerson1;
        arrayOfPersons[1] = newPerson2;
        arrayOfPersons[2] = newPerson3;

        _addNewSet(arrayOfPersons);
    }

    function _addNewSet(Person[] memory _persons) internal {
        uint len = _persons.length;
        unchecked {
            for(uint i; i < len; ++i) {
                Person memory newStudent = Person(_persons[i].name, _persons[i].age, _persons[i].id);
                students.push(newStudent);
            }
        }

    }
}
```
```
test for test/NoCacheArrayElement.t.sol:NoCacheArrayElementTest
[PASS] test_createStudents() (gas: 230357)
```

```solidity

struct Person {
    string name;
    uint age;
    uint id;
}

contract CacheArrayElement {

    Person[] students;

    function createStudents() external  {
        Person[] memory arrayOfPersons = new Person[](3); 
        Person memory newPerson1 = Person("Emmanuel", 15,1);
        Person memory newPerson2 = Person("Faustina", 16,2);
        Person memory newPerson3 = Person("Emmanuela", 14,3);

        arrayOfPersons[0] = newPerson1;
        arrayOfPersons[1] = newPerson2;
        arrayOfPersons[2] = newPerson3;

        _addNewSet(arrayOfPersons);
    }

    function _addNewSet(Person[] memory _persons) internal {
        uint len = _persons.length;
        unchecked {
            for(uint i; i < len; ++i) {
                Person memory myPerson = _persons[i];
                Person memory newStudent = Person(myPerson.name, myPerson.age, myPerson.id);
                students.push(newStudent);
            }
        }

    }
}
```
```
test for test/Counter.t.sol:CacheArrayElementTest
[PASS] test_createStudents() (gas: 230096)
```

### 2 Instances
1. #### Cache `hooks[i]` to avoid re-calculating the array offsets into memory
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L477&&#L485


```solidity
file: src/policies/Create.sol

464:    function _addHooks(
465:        Hook[] memory hooks,
466:        SpentItem[] memory offerItems,
467:        address rentalWallet
468:    ) internal {
469:        // Define hook target, offer item index, and an offer item.
470:        address target;
471:        uint256 itemIndex;
472:        SpentItem memory offer;
473:
474:        // Loop through each hook in the payload.
475:        for (uint256 i = 0; i < hooks.length; ++i) {
476:            // Get the hook's target address.
477:            target = hooks[i].target;   //@audit cache hooks[i];
478:
479:            // Check that the hook is reNFT-approved to execute on rental start.
480:            if (!STORE.hookOnStart(target)) {
481:                revert Errors.Shared_DisabledHook(target);
482:            }
483:
484:            // Get the offer item index for this hook.
485:            itemIndex = hooks[i].itemIndex;   //@audit cache hooks[i];
486:
487:            // Get the offer item for this hook.
488:            offer = offerItems[itemIndex];
489:
490:            // Make sure the offer item is an ERC721 or ERC1155.
491:            if (!offer.isRental()) {
492:                revert Errors.Shared_NonRentalHookItem(itemIndex);
493:            }
494:
495:            // Call the hook with data about the rented item.
496:            try
497:                IHook(target).onStart(
498:                    rentalWallet,
499:                    offer.token,
500:                    offer.identifier,
501:                    offer.amount,
502:                    hooks[i].extraData   //@audit cache hooks[i];
503:                )
.
.
.
520:    }
```

```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..b2f4241 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -470,11 +470,12 @@ contract Create is Policy, Signer, Zone, Accumulator {
         address target;
         uint256 itemIndex;
         SpentItem memory offer;
-
+        Hook memory hook;
         // Loop through each hook in the payload.
         for (uint256 i = 0; i < hooks.length; ++i) {
             // Get the hook's target address.
-            target = hooks[i].target;
+            hook = hooks[i];
+            target = hook.target;

             // Check that the hook is reNFT-approved to execute on rental start.
             if (!STORE.hookOnStart(target)) {
@@ -482,7 +483,7 @@ contract Create is Policy, Signer, Zone, Accumulator {
             }

             // Get the offer item index for this hook.
-            itemIndex = hooks[i].itemIndex;
+            itemIndex = hook.itemIndex;

             // Get the offer item for this hook.
             offer = offerItems[itemIndex];
@@ -499,7 +500,7 @@ contract Create is Policy, Signer, Zone, Accumulator {
                     offer.token,
                     offer.identifier,
                     offer.amount,
-                    hooks[i].extraData
+                    hook.extraData
                 )
             {} catch Error(string memory revertReason) {
                 // Revert with reason given.
```


2. #### Cache `items[i]` to avoid re-calculating the array offsets into memory
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L568-#L573
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L600-#L601

```solidity
file: src/policies/Create.sol

530:    function _rentFromZone(
531:        RentPayload memory payload,
532:        SeaportPayload memory seaportPayload
533:    ) internal {
534:        // Check: make sure order metadata is valid with the given seaport order zone hash.
535:        _isValidOrderMetadata(payload.metadata, seaportPayload.zoneHash);
536:
537:        // Check: verify the fulfiller of the order is an owner of the recipient safe.
538:        _isValidSafeOwner(seaportPayload.fulfiller, payload.fulfillment.recipient);
539:
540:        // Check: verify each execution was sent to the expected destination.
541:        _executionInvariantChecks(
542:            seaportPayload.totalExecutions,
543:            payload.fulfillment.recipient
544:        );
545:
546:        // Check: validate and process seaport offer and consideration items based
547:        // on the order type.
548:        Item[] memory items = _convertToItems(
549:            seaportPayload.offer,
550:            seaportPayload.consideration,
551:            payload.metadata.orderType
552:        );
553:
.
.
.
567:            for (uint256 i; i < items.length; ++i) {
568:                if (items[i].isRental()) {  //@audit cache items[i]
569:                    // Insert the rental asset update into the dynamic array.
570:                    _insert(
571:                        rentalAssetUpdates,
572:                        items[i].toRentalId(payload.fulfillment.recipient),  //@audit cache items[i]
573:                        items[i].amount  //@audit cache items[i]
574:                    );
575:                }
576:            }
.
.
.
599:            for (uint256 i = 0; i < items.length; ++i) {
600:                if (items[i].isERC20()) {  //@audit cache items[i]
601:                    ESCRW.increaseDeposit(items[i].token, items[i].amount);  //@audit cache items[i]
602:                }
603:            }
.
.
.
617:    }
```

```diff
diff --git a/src/policies/Create.sol b/src/policies/Create.sol
index 061180d..b5ddcbf 100644
--- a/src/policies/Create.sol
+++ b/src/policies/Create.sol
@@ -561,16 +561,17 @@ contract Create is Policy, Signer, Zone, Accumulator {
             // the rented amount. From this point on, new memory cannot be safely allocated until the
             // accumulator no longer needs to include elements.
             bytes memory rentalAssetUpdates = new bytes(0);
-
+            Item memory item;
             // Check if each item is a rental. If so, then generate the rental asset update.
             // Memory will become safe again after this block.
             for (uint256 i; i < items.length; ++i) {
-                if (items[i].isRental()) {
+                item = items[i];
+                if (item.isRental()) {
                     // Insert the rental asset update into the dynamic array.
                     _insert(
                         rentalAssetUpdates,
-                        items[i].toRentalId(payload.fulfillment.recipient),
-                        items[i].amount
+                        item.toRentalId(payload.fulfillment.recipient),
+                        item.amount
                     );
                 }
             }
@@ -597,8 +598,9 @@ contract Create is Policy, Signer, Zone, Accumulator {
             // Interaction: Increase the deposit value on the payment escrow so
             // it knows how many tokens were sent to it.
             for (uint256 i = 0; i < items.length; ++i) {
-                if (items[i].isERC20()) {
-                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
+                item = items[i];
+                if (item.isERC20()) {
+                    ESCRW.increaseDeposit(item.token, item.amount);
                 }
             }
```





## [G-08]  State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variables.

### 5 Instances

1. #### Refactor `Admin.requestPermissions()`  to avoid 6 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L74-#L81
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L83-#L86

We can reduce the gas cost of the `requestPermissions()` function by reducing the number of storage reads (`SLOAD`) for the state variables `STORE` and `ESCRW`. The value of the `STORE` and `ESCRW` state variables should be cached stack variables then the stack variables be used for subsequent reads of the `STORE` and `ESCRW` state variable. In implementing this we replace 6 `SLOAD`s(cold acess) `100` gas units with  much cheaper 6 `MLOAD` `18` gas units: The diff below shows how the code could be refactored:

```solidity
file: src/policies/Admin.sol

64:    function requestPermissions()
65:        external
66:        view
67:        override
68:        onlyKernel
69:        returns (Permissions[] memory requests)
70:    {
71:        requests = new Permissions[](8);
72:        requests[0] = Permissions(
73:            toKeycode("STORE"),
74:            STORE.toggleWhitelistExtension.selector  //@audit STORE 1st SLOAD
75:        );
76:        requests[1] = Permissions(
77:            toKeycode("STORE"),
78:            STORE.toggleWhitelistDelegate.selector  //@audit STORE 2nd SLOAD
79:        );
80:        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);  //@audit STORE 3rd SLOAD
81:        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);  //@audit STORE 4th SLOAD
82:
83:        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);  //@audit ESCRW 1st SLOAD
84:        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);  //@audit ESCRW 2nd SLOAD
85:        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);  //@audit ESCRW 3rd SLOAD
86:        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);  //@audit ESCRW 4th SLOAD
87:    }
```
```diff
diff --git a/src/policies/Admin.sol b/src/policies/Admin.sol
index b37d47f..4e277f7 100644
--- a/src/policies/Admin.sol
+++ b/src/policies/Admin.sol
@@ -69,21 +69,23 @@ contract Admin is Policy {
         returns (Permissions[] memory requests)
     {
         requests = new Permissions[](8);
+        Storage _store = STORE;
+        PaymentEscrow _escrw = ESCRW;
         requests[0] = Permissions(
             toKeycode("STORE"),
-            STORE.toggleWhitelistExtension.selector
+            _store.toggleWhitelistExtension.selector
         );
         requests[1] = Permissions(
             toKeycode("STORE"),
-            STORE.toggleWhitelistDelegate.selector
+            _store.toggleWhitelistDelegate.selector
         );
-        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
-        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);
+        requests[2] = Permissions(toKeycode("STORE"), _store.upgrade.selector);
+        requests[3] = Permissions(toKeycode("STORE"), _store.freeze.selector);

-        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
-        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
-        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
-        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
+        requests[4] = Permissions(toKeycode("ESCRW"), _escrw.skim.selector);
+        requests[5] = Permissions(toKeycode("ESCRW"), _escrw.setFee.selector);
+        requests[6] = Permissions(toKeycode("ESCRW"), _escrw.upgrade.selector);
+        requests[7] = Permissions(toKeycode("ESCRW"), _escrw.freeze.selector);
     }
```
```
Estimated gas saved: 582 gas units
```

2. #### Refactor `Factory.deployRentalSafe()`  to avoid 1 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L189

We can reduce the gas cost of the `deployRentalSafe()` function by reducing the number of storage reads (`SLOAD`) for the state variable `STORE`. The value of the `STORE` state variable should be cached in a stack variable then the stack variable be used for subsequent reads of the `STORE` state variable. In implementing this we replace 1 `SLOAD`s(cold acess) `100` gas units with  much cheaper 1 `MLOAD` `3` gas units: The diff below shows how the code could be refactored:

```solidity
file: src/policies/Factory.sol

138:    function deployRentalSafe(
139:        address[] calldata owners,
140:        uint256 threshold
141:    ) external returns (address safe) {
142:        // Require that the threshold is valid.
143:        if (threshold == 0 || threshold > owners.length) {
144:            revert Errors.FactoryPolicy_InvalidSafeThreshold(threshold, owners.length);
145:        }
.
.
.
180:        safe = address(
181:            safeProxyFactory.createProxyWithNonce(
182:                address(safeSingleton),
183:                initializerPayload,
184:                uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))  //@audit STORE 1st SLOAD
185:            )
186:        );
187:
188:        // Store the deployed safe.
189:        STORE.addRentalSafe(safe);  //@audit STORE 2nd SLOAD
190:
191:        // Emit the event.
192:        emit Events.RentalSafeDeployment(safe, owners, threshold);
193:    }
```
```diff
diff --git a/src/policies/Factory.sol b/src/policies/Factory.sol
index 8401ad7..f019f3c 100644
--- a/src/policies/Factory.sol
+++ b/src/policies/Factory.sol
@@ -174,6 +174,7 @@ contract Factory is Policy {
             )
         );

+        Storage _store = STORE;
         // Deploy a safe proxy using initializer values for the Safe.setup() call
         // with a salt nonce that is unique to each chain to guarantee cross-chain
         // unique safe addresses.
@@ -181,12 +182,12 @@ contract Factory is Policy {
             safeProxyFactory.createProxyWithNonce(
                 address(safeSingleton),
                 initializerPayload,
-                uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))
+                uint256(keccak256(abi.encode(_store.totalSafes() + 1, block.chainid)))
             )
         );

         // Store the deployed safe.
-        STORE.addRentalSafe(safe);
+        _store.addRentalSafe(safe);

         // Emit the event.
         emit Events.RentalSafeDeployment(safe, owners, threshold);
```
```
Estimated gas saved: 97
```


3. #### Refactor `Guard.requestPermissions()`  to avoid 1 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L93

We can reduce the gas cost of the `requestPermissions()` function by reducing the number of storage reads (`SLOAD`) for the state variable `STORE`. The value of the `STORE` state variable should be cached in a stack variable then the stack variable be used for subsequent reads of the `STORE` state variable. In implementing this we replace 1 `SLOAD`s(cold acess) `100` gas units with  much cheaper 1 `MLOAD` `3` gas units: The diff below shows how the code could be refactored:

```solidity
file: src/policies/Guard.sol

84:    function requestPermissions()
85:        external
86:        view
87:        override
88:        onlyKernel
89:        returns (Permissions[] memory requests)
90:    {
91:        requests = new Permissions[](2);
92:        requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);  //@audit STORE 1st SLOAD
93:        requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);  //@audit STORE 2nd SLOAD
94:    }
```

```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index a0a565a..053e45e 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -88,9 +88,10 @@ contract Guard is Policy, BaseGuard {
         onlyKernel
         returns (Permissions[] memory requests)
     {
+        Storage _store = STORE;
         requests = new Permissions[](2);
-        requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);
-        requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);
+        requests[0] = Permissions(toKeycode("STORE"), _store.updateHookPath.selector);
+        requests[1] = Permissions(toKeycode("STORE"), _store.updateHookStatus.selector);
     }

     /////////////////////////////////////////////////////////////////////////////////
```
```
Estimated gas saved: 97 gas units
```

4. #### Refactor `Guard.checkTransaction()`  to avoid 2 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L335

We can reduce the gas cost of the `checkTransaction()` function by reducing the number of storage reads (`SLOAD`) for the state variable `STORE`. The value of the `STORE` state variable should be cached in a stack variable then the stack variable be used for subsequent reads of the `STORE` state variable. In implementing this we replace 2 `SLOAD`s(cold acess) `200` gas units with  much cheaper 2 `MLOAD` `6` gas units: The diff below shows how the code could be refactored:

```solidity
file: src/policies/Guard.sol

309:    function checkTransaction(
310:        address to,
311:        uint256 value,
312:        bytes memory data,
313:        Enum.Operation operation,
314:        uint256,
315:        uint256,
316:        uint256,
317:        address,
318:       address payable,
319:        bytes memory,
320:        address
321:    ) external override {
322:        // Disallow transactions that use delegate call, unless explicitly
323:        // permitted by the protocol.
324:        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {  //@audit STORE 1st SLOAD
325:            revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
326:        }
327:
328:        // Require that a function selector exists.
329:        if (data.length < 4) {
330:            revert Errors.GuardPolicy_FunctionSelectorRequired();
331:        }
332:
333:        // Fetch the hook to interact with for this transaction.
334:        address hook = STORE.contractToHook(to);  //@audit STORE 2nd SLOAD
335:        bool isActive = STORE.hookOnTransaction(hook);  //@audit STORE 3rd SLOAD
.
.
.
345:    }
```

```diff
diff --git a/src/policies/Guard.sol b/src/policies/Guard.sol
index a0a565a..c24619f 100644
--- a/src/policies/Guard.sol
+++ b/src/policies/Guard.sol
@@ -319,9 +319,11 @@ contract Guard is Policy, BaseGuard {
         bytes memory,
         address
     ) external override {
+
+        Storage _store = STORE;
         // Disallow transactions that use delegate call, unless explicitly
         // permitted by the protocol.
-        if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
+        if (operation == Enum.Operation.DelegateCall && !_store.whitelistedDelegates(to)) {
             revert Errors.GuardPolicy_UnauthorizedDelegateCall(to);
         }

@@ -331,8 +333,8 @@ contract Guard is Policy, BaseGuard {
         }

         // Fetch the hook to interact with for this transaction.
-        address hook = STORE.contractToHook(to);
-        bool isActive = STORE.hookOnTransaction(hook);
+        address hook = _store.contractToHook(to);
+        bool isActive = _store.hookOnTransaction(hook);

         // If a hook exists and is enabled, forward the control flow to the hook.
         if (hook != address(0) && isActive) {
```
```
Estimated gas saved: 194 gas units saved
```

5. #### Refactor `Stop.requestPermissions()`  to avoid 2 `SLOAD(Gwarmaccess)`
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L96
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L98

We can reduce the gas cost of the `requestPermissions()` function by reducing the number of storage reads (`SLOAD`) for the state variables `STORE` and `ESCRW`. The value of the `STORE` and `ESCRW` state variables should be cached stack variables then the stack variables be used for subsequent reads of the `STORE` and `ESCRW` state variable. In implementing this we replace 2 `SLOAD`s(cold acess) `200` gas units with  much cheaper 2 `MLOAD` `6` gas units: The diff below shows how the code could be refactored:

```solidity
file: src/policies/Stop.sol

87:    function requestPermissions()
88:        external
89:        view
90:        override
91:        onlyKernel
92:        returns (Permissions[] memory requests)
93:    {
94:        requests = new Permissions[](4);
95:        requests[0] = Permissions(toKeycode("STORE"), STORE.removeRentals.selector);  //@audit STORE 1st SLOAD
96:        requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);  //@audit STORE 2nd SLOAD
97:        requests[2] = Permissions(toKeycode("ESCRW"), ESCRW.settlePayment.selector);  //@audit ESCRW 1st SLOAD
98:        requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);  //@audit ESCRW 2nd SLOAD
99:    }
```

```diff
diff --git a/src/policies/Stop.sol b/src/policies/Stop.sol
index ab240ba..ad7cee5 100644
--- a/src/policies/Stop.sol
+++ b/src/policies/Stop.sol
@@ -92,10 +92,13 @@ contract Stop is Policy, Signer, Reclaimer, Accumulator {
         returns (Permissions[] memory requests)
     {
         requests = new Permissions[](4);
-        requests[0] = Permissions(toKeycode("STORE"), STORE.removeRentals.selector);
-        requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);
-        requests[2] = Permissions(toKeycode("ESCRW"), ESCRW.settlePayment.selector);
-        requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);
+        Storage _store = STORE;
+        PaymentEscrow _escrw = ESCRW;
+
+        requests[0] = Permissions(toKeycode("STORE"), _store.removeRentals.selector);
+        requests[1] = Permissions(toKeycode("STORE"), _store.removeRentalsBatch.selector);
+        requests[2] = Permissions(toKeycode("ESCRW"), _escrw.settlePayment.selector);
+        requests[3] = Permissions(toKeycode("ESCRW"), _escrw.settlePaymentBatch.selector);
     }
```
```
Estimated gas saved: 194 gas units
```





## CONCLUSION
As you embark on incorporating the recommended optimizations, we want to emphasize the utmost importance of proceeding with vigilance and dedicating thorough efforts to comprehensive testing. It is of paramount significance to ensure that the proposed alterations do not inadvertently introduce fresh vulnerabilities, while also successfully achieving the anticipated enhancements in performance.

We strongly advise conducting a meticulous and exhaustive evaluation of the modifications made to the codebase. This rigorous scrutiny and exhaustive assessment will play a pivotal role in affirming both the security and efficacy of the refactored code. Your careful attention to detail, coupled with the implementation of a robust testing framework, will provide the necessary assurance that the refined code aligns with your security objectives and effectively fulfills the intended performance optimizations.
