## Gas Optimizations

| Number                                                                                        | Issue                                                                            
| --------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------- 
| [[G-01](#g-01-pack-structs-by-putting-variables-that-can-fit-together-next-to-each-other-saves-4000-gas)]   | Pack `structs` by putting variables that can fit together next to each other (saves 4000 Gas)                             
| [[G-02](#g-02-cache-the-function-call-instead-of-re-calling-them)] | `Cache` the function call instead of re-calling them 
| [[G-03](#g-03-avoid-writing-storage-variable-with-same-value-not-in-bot-report)]                                                 | Avoid writing `storage` variable with same value (Not in bot report)                                                    
| [[G-04](#g-04-no-need-to-cache-function-call-if-called-only-once)]                | No need to `cache` Function call if called only once                   
| [[G-05](#g-05-dont-cache-state-variable-if-it-is-used-only-once)]                | Don't cache `state variable` if it is used only once                   



## [G-01] Pack `structs` by putting variables that can fit together next to each other (saves 4000 Gas)

### `SAVE: 4000 GAS, 2 SLOT`

### Note: This packing is in `RentalStructs.sol` file which is out of scope. But I have added that since this is used by in scope contracts and it's struct variables declared here. Occupying the storage slots of in scope of contracts. By improving size in that file  can save slots actually in contracts. And saves lot of gas in scope contracts.

As the solidity EVM works with 32 bytes, variables less than 32 bytes should be packed inside a struct so that they can be stored in the same slot, this saves gas when writing to storage ~20000 gas

### truncate `startTimestamp` and `endTimestamp` to uint40 and pack with `rentalWallet` :`SAVES 4000 GAS, 2 SLOT`

The uint40 type is safe for approximately 1.099511627776e+12 years. The exact number of years that a uint40 type is safe for is 1099511627776 years.

```diff
File : src/libraries/RentalStructs.sol

struct RentalOrder {
    bytes32 seaportOrderHash;
    Item[] items;
    Hook[] hooks;
    OrderType orderType;
    address lender;
    address renter;
    address rentalWallet;
+    uint40 startTimestamp;
+    uint40 endTimestamp;
-    uint256 startTimestamp;
-    uint256 endTimestamp;
}

```
[133-143](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalStructs.sol#L133C1-L143C2)

## [G-02] `Cache` the function call instead of re-calling them

_16 Instances_

### Result of function `toKeycode("STORE")` and `toKeycode("ESCRW")` should be cached instead of re-calling them


```solidity
File : src/policies/Admin.sol

    function configureDependencies()
        external
        override
        onlyKernel
        returns (Keycode[] memory dependencies)
    {
        dependencies = new Keycode[](2);

        dependencies[0] = toKeycode("STORE");
        STORE = Storage(getModuleAddress(toKeycode("STORE")));

        dependencies[1] = toKeycode("ESCRW");
        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
    }

    ...

    function requestPermissions()
        external
        view
        override
        onlyKernel
        returns (Permissions[] memory requests)
    {
        requests = new Permissions[](8);
        requests[0] = Permissions(
            toKeycode("STORE"),
            STORE.toggleWhitelistExtension.selector
        );
        requests[1] = Permissions(
            toKeycode("STORE"),
            STORE.toggleWhitelistDelegate.selector
        );
        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);

        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
    }

```
[40-53](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L40C1-L53C6), [64-87](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L64C5-L87C6)

```diff

-import {toKeycode} from "@src/libraries/KernelUtils.sol";
+import {toKeycode, keycode} from "@src/libraries/KernelUtils.sol";
...
    function configureDependencies()
        external
        override
        onlyKernel
        returns (Keycode[] memory dependencies)
    {
        dependencies = new Keycode[](2);

+       keycode _tokeycodeSTORE = toKeycode("STORE");        
+       keycode _tokeycodeESCRW = toKeycode("ESCRW");        

-        dependencies[0] = toKeycode("STORE");
-        STORE = Storage(getModuleAddress(toKeycode("STORE")));
-
-        dependencies[1] = toKeycode("ESCRW");
-        ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));

+        dependencies[0] = _tokeycodeSTORE;
+        STORE = Storage(getModuleAddress(_tokeycodeSTORE));
+  
+        dependencies[1] = _tokeycodeESCRW;
+        ESCRW = PaymentEscrow(getModuleAddress(_tokeycodeESCRW));
    }

    ...

    function requestPermissions()
        external
        view
        override
        onlyKernel
        returns (Permissions[] memory requests)
    {

+       keycode _tokeycodeSTORE = toKeycode("STORE");        
+       keycode _tokeycodeESCRW = toKeycode("ESCRW");

        requests = new Permissions[](8);
-        requests[0] = Permissions(
-            toKeycode("STORE"),
-            STORE.toggleWhitelistExtension.selector
-        );
-        requests[1] = Permissions(
-            toKeycode("STORE"),
-            STORE.toggleWhitelistDelegate.selector
-        );
-        requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
-        requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);

-        requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);
-        requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
-        requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
-        requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);

+        requests[0] = Permissions(
+            _tokeycodeSTORE,
+            STORE.toggleWhitelistExtension.selector
+        );
+        requests[1] = Permissions(
+            _tokeycodeSTORE,
+            STORE.toggleWhitelistDelegate.selector
+        );
+        requests[2] = Permissions(_tokeycodeSTORE, STORE.upgrade.selector);
+        requests[3] = Permissions(_tokeycodeSTORE, STORE.freeze.selector);

+        requests[4] = Permissions(_tokeycodeESCRW, ESCRW.skim.selector);
+        requests[5] = Permissions(_tokeycodeESCRW, ESCRW.setFee.selector);
+        requests[6] = Permissions(_tokeycodeESCRW, ESCRW.upgrade.selector);
+        requests[7] = Permissions(_tokeycodeESCRW, ESCRW.freeze.selector);
    }

```

### Result of function `toKeycode("STORE")` should be cached

```solidity
File : src/policies/Guard.sol

import {toKeycode} from "@src/libraries/KernelUtils.sol";
...
    function configureDependencies()
        external
        override
        onlyKernel
        returns (Keycode[] memory dependencies)
    {
        dependencies = new Keycode[](1);

        dependencies[0] = toKeycode("STORE");
        STORE = Storage(getModuleAddress(toKeycode("STORE")));
    }

...
    function requestPermissions()
        external
        view
        override
        onlyKernel
        returns (Permissions[] memory requests)
    {
        requests = new Permissions[](2);
        requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);
        requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);
    }


```
[10-95](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L10C1-L95C1)

```diff
File : src/policies/Guard.sol

-import {toKeycode} from "@src/libraries/KernelUtils.sol";
+import {toKeycode,keycode} from "@src/libraries/KernelUtils.sol";
...
    function configureDependencies()
        external
        override
        onlyKernel
        returns (Keycode[] memory dependencies)
    {
        dependencies = new Keycode[](1);

+       keycode _tokeycodeSTORE = toKeycode("STORE");        


-        dependencies[0] = toKeycode("STORE");
-        STORE = Storage(getModuleAddress(toKeycode("STORE")));

+        dependencies[0] = _tokeycodeSTORE;
+        STORE = Storage(getModuleAddress(_tokeycodeSTORE));
    }

...
    function requestPermissions()
        external
        view
        override
        onlyKernel
        returns (Permissions[] memory requests)
    {
        requests = new Permissions[](2);

+       keycode _tokeycodeSTORE = toKeycode("STORE"); 

-        requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);
-        requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);
+        requests[0] = Permissions(_tokeycodeSTORE, STORE.updateHookPath.selector);
+        requests[1] = Permissions(_tokeycodeSTORE, STORE.updateHookStatus.selector);
    }


```

## [G-03] Avoid writing `storage` variable with same value (Not in bot report)

In Solidity, optimizing gas usage involves avoiding unnecessary storage updates when the new value equals the existing one. This can save significant gas, around 2900 for 'Gsreset.' However, this may result in a 2100 gas cost for 'Gcoldsload' or a 100 gas cost for 'Gwarmaccess.'

 **To enhance gas efficiency, add a check to compare new and current values before any storage update: bypass the operation if they are the same.**

_1 Instance_

 ```solidity
 File : src/Kernel.sol

192: function setActiveStatus(bool activate_) external onlyKernel {
193:        isActive = activate_;
194:    }
 
 ```
 [192-194](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L192C5-L194C6)

## [G-04] No need to `cache` Function call if called only once

### Don't cache `_calculateFee(paymentAmount)` 

```solidity
File : src/modules/PaymentEscrow.sol

242:  if (fee != 0) {
243:  // Calculate the new fee.
244:  uint256 paymentFee = _calculateFee(paymentAmount);
245:
246:  // Adjust the payment amount by the fee.
247:   paymentAmount -= paymentFee;
248:  }

```
[242-248](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L242C1-L248C18)

```diff
File : src/modules/PaymentEscrow.sol

    if (fee != 0) {
        // Calculate the new fee.
-        uint256 paymentFee = _calculateFee(paymentAmount);

            // Adjust the payment amount by the fee.
-            paymentAmount -= paymentFee;
+            paymentAmount -= _calculateFee(paymentAmount);
        }

```

### Don't cache `0`

```solidity
File : src/modules/PaymentEscrow.sol

402:  uint256 trueBalance = IERC20(token).balanceOf(address(this));

```
[402](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L402)

```diff
File : src/modules/PaymentEscrow.sol

function skim(address token, address to) external onlyByProxy permissioned {
        // Fetch the currently synced balance of the escrow.
        uint256 syncedBalance = balanceOf[token];

        // Fetch the true token balance of the escrow.
-        uint256 trueBalance = IERC20(token).balanceOf(address(this));

        // Calculate the amount to skim.
-        uint256 skimmedBalance = trueBalance - syncedBalance;
+        uint256 skimmedBalance = IERC20(token).balanceOf(address(this)) - syncedBalance;

        // Send the difference to the specified address.
        _safeTransfer(token, to, skimmedBalance);

        // Emit event with fees taken.
        emit Events.FeeTaken(token, skimmedBalance);
    }

```

## [G-05] Don't cache `state variable` if it is used only once

### Don't cache `balanceOf[token]`

```solidity
File : src/modules/PaymentEscrow.sol

399: uint256 syncedBalance = balanceOf[token];

```
[397-412](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L397C5-L412C6)

```diff
File : src/modules/PaymentEscrow.sol

function skim(address token, address to) external onlyByProxy permissioned {
        // Fetch the currently synced balance of the escrow.
-       uint256 syncedBalance = balanceOf[token];

        // Fetch the true token balance of the escrow.
        uint256 trueBalance = IERC20(token).balanceOf(address(this));

        // Calculate the amount to skim.
-        uint256 skimmedBalance = trueBalance - syncedBalance;
+        uint256 skimmedBalance = trueBalance - balanceOf[token];

        // Send the difference to the specified address.
        _safeTransfer(token, to, skimmedBalance);

        // Emit event with fees taken.
        emit Events.FeeTaken(token, skimmedBalance);
    }

```