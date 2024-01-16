# 1. `changeKernel` function should check if newKernel == oldkernel
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L54
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L516
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L529
### Impact
Kernel change carries the risk of bricking current in-use kernel. Switching to the the same kernel might cause unexpected behaviours and lock out permissioned functions from the modules and policies.

### Recommended Mitigation Steps
Add a same address check to the change kernel function.
```
         if (address(newKernel) == address(0) || oldKernel == newKernel_) { 
            revert InvalidKernelChange;
        }
```

# 2. `executeAction` doesn't provide an error for a non existent action. 
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L277C1-L302C6
### Impact
The current implementation emits an event irrespective of whether the action to be performed exists or not. Consider fixing this by introducing a reversion. 
### Recommended Mitigation Steps
``` 
        function executeAction(Actions action_, address target_) external onlyExecutor {
            ...
                else {
                    revert NonExistentAction;
        }
        }
```


# 3. It's possible to activate a policy multiple times
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L420
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L458

### Impact
Due to the reliance of the `_activatePolicy` function on the return value of an external contract, a malicious policy that returns false on the `isActive` check can easily be readded. If activated enough times, the now large policy and dependencies array can be used to DOS the protocol during kernel migrations and policy reconfigurations. 
```
    function _activatePolicy(Policy policy_) internal {
        // Ensure that the policy is not already active.
        if (policy_.isActive())
            revert Errors.Kernel_PolicyAlreadyApproved(address(policy_));
```

Important to note that such policies can also not be deactivated because the `_deactivatePolicy` function will revert when the policy active check returns false.
```
    function _deactivatePolicy(Policy policy_) internal {
        if (!policy_.isActive()) revert Errors.Kernel_PolicyNotApproved(address(policy_));
```
### Recommended Mitigation Steps
Recommend checking for policy's index instead. 
```
    function _activatePolicy(Policy policy_) internal {
        // Ensure that the policy is not already active.

        if (getPolicyIndex[policy_] != 0) revert Errors.Kernel_PolicyAlreadyApproved

        ...

    }

```
```
    function _deactivatePolicy(Policy policy_) internal {

        if (getPolicyIndex[policy_] = 0) revert Errors.Kernel_PolicyNotApproved

        ...

    }

```

# 4. `_deactivatePolicy` lacks checks for policy existence
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L457C5-L490C6
### Impact
Trying to remove a non existent policy will result in the policy at the 0th index being removed. This affects the functionality of the protocl and can bring about downtown.

### Recommended Mitigation Steps
Comnsider introducing a check for existing policiy
```
if (activePolicies[idx] != policy) revert Errors.Kernel_PolicyNotRegistered
```


# 5. Address Collision Risk in `getCreate2Address` function`
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L84
### Impact
The `getCreate2Address` function uses deterministic address computation to determine the `targetDeploymentAddress` of the contract to the deployed. This approach is based on the assumption that the address derived from the contract's creation bytecode and the creator's address is unique. However, this method is susceptible to address collision risks ([a birthday paradox](https://www.scientificamerican.com/article/bring-science-home-probability-birthday-paradox/)), where a different contract, under specific circumstances, might share the same computed address, leading to erroneous contract validation and deploymnet.

### Recommended Mitigation Steps
Improve the validation logic by incorporating additional checks beyond address computation. 



# 6. `.freeze` functions should ensure that existing module operations are completely executed before freezing
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L135
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L155
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L431
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L371
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/proxy/Proxiable.sol#L110C1-L115C6
### Impact
Freezing modules are irreversible, and the `freeze` functions do not check if all module functions have been completed before freezing.
Freezing the `PaymentEscrow` for instance will result in any unskimmed funds being lost. Freezing the `Storage` will result in rentals being locked in the protocols.

### Recommended Mitigation Steps
Consider implementing the `removeRentals` and `skim` functions in the `freeze` implementations.

# 7. Rental safes can be added but can't be removed
Links to affected code *
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L274C1-L283C6
### Impact
Safe can only be added not removed. This is not good practice as vulnerable rental safes which might pose a threat to the protcol's functionality will be unremovable.

### Recommended Mitigation Steps
Implement a removeSafe function.