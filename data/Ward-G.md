## G-1 Replace `modifier` with `function`
Modifiers make code more elegant, but cost more than normal functions
All modifiers except `permissioned` due to unresolved error flow

src/Kernel.sol:[40-44](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L40-L44), [130-136](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L130-L136), [254-265](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L254-L265)
```solidity
    modifier onlyKernel() {
        if (msg.sender != address(kernel)) {
            revert Errors.KernelAdapter_OnlyKernel(msg.sender);
        }
        _;
    }


    modifier onlyRole(bytes32 role_) {
        Role role = toRole(role_);
        if (!kernel.hasRole(msg.sender, role)) {
            revert Errors.Policy_OnlyRole(role);
        }
        _;
    }

    modifier onlyExecutor() {
        if (msg.sender != executor) revert Errors.Kernel_OnlyExecutor(msg.sender);
        _;
    }

    /**
     * @dev modifier which only allows calls by an admin address.
     */
    modifier onlyAdmin() {
        if (msg.sender != admin) revert Errors.Kernel_OnlyAdmin(msg.sender);
        _;
    }
```

## G-2 `storage` pointer to a structure is cheaper than copying each value of the structure into `memory`, same for `array` and `mapping`

It may not be obvious, but every time you copy a storage `struct`/`array`/`mapping` to a memory variable, you are literally copying each member by reading it from `storage`, which is expensive. And when you use the `storage` keyword, you are just storing a pointer to the storage, which is much cheaper.

src/Kernel.sol [540-550](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L540-L550)

```solidity
function _reconfigurePolicies(Keycode keycode_) internal {
        // Get an array of all policies that depend on the keycode.
        Policy[] memory dependents = moduleDependents[keycode_];
        uint256 depLength = dependents.length;

        // Loop through each policy.
        for (uint256 i; i < depLength; ++i) {
            // Reconfigure its dependencies.
            dependents[i].configureDependencies();
        }
    }
```

fix

```diff
function _reconfigurePolicies(Keycode keycode_) internal {
        // Get an array of all policies that depend on the keycode.
-        Policy[] memory dependents = moduleDependents[keycode_];
+        Policy[] storage dependents = moduleDependents[keycode_];
        uint256 depLength = dependents.length;

        // Loop through each policy.
        for (uint256 i; i < depLength; ++i) {
            // Reconfigure its dependencies.
            dependents[i].configureDependencies();
        }
    }
```

## G-3 Use named `returns` for local variables where it is possible

src/Kernel.sol [180-185](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L180-L185)

```solidity
    function getModuleAddress(Keycode keycode_) internal view returns (address) {
        address moduleForKeycode = address(kernel.getModuleForKeycode(keycode_));
        if (moduleForKeycode == address(0)) revert Policy_ModuleDoesNotExist(keycode_);
        return moduleForKeycode;
    }
```

fix

```solidity
    function getModuleAddress(Keycode keycode_) internal view returns (address moduleForKeycode) {
        moduleForKeycode = address(kernel.getModuleForKeycode(keycode_));
        if (moduleForKeycode == address(0)) revert Policy_ModuleDoesNotExist(keycode_);
    }
```
