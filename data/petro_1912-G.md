There are unnecessary reading and writing to the storage slots.
While deactivating a policy, contract find the index of item being deactivated and bring the last item to that index.
If the item being deactivated is the last item in the array, then no need to access the storage slot, but just need to pop.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L469-L479

Kernel.sol
```
function _deactivatePolicy(Policy policy_) internal {
        if (!policy_.isActive()) revert Errors.Kernel_PolicyNotApproved(address(policy_));

        // Fetch originally granted permissions from the policy
        // and then revoke them.
        Permissions[] memory requests = policy_.requestPermissions();
        _setPolicyPermissions(policy_, requests, false);

        // Get the index of the policy in the active policies array.
        uint256 idx = getPolicyIndex[policy_];

        // Get the index of the last policy in the active policy array.
469:    Policy lastPolicy = activePolicies[activePolicies.length - 1];

        // Set the last policy at the index of the policy to deactivate.
472:    activePolicies[idx] = lastPolicy;

        // Pop the last policy from the array.
        activePolicies.pop();

        // Set the last policy's index to the index of the policy
        // that was removed.
479:    getPolicyIndex[lastPolicy] = idx;

        // Delete the index of the policy being deactivated.
        delete getPolicyIndex[policy_];

        // Remove policy from array of dependents for each keycode
        // that the policy depends upon.
        _pruneFromDependents(policy_);

        // Set policy status to inactive.
        policy_.setActiveStatus(false);
}
```

As we can see L469, L472, L479, it is unnecessary if the `idx` is `activePolicies.length - 1`;
We can rewrite the `_deactivatePolicy` like this:

```
function _deactivatePolicy(Policy policy_) internal {
        if (!policy_.isActive()) revert Errors.Kernel_PolicyNotApproved(address(policy_));

        // Fetch originally granted permissions from the policy
        // and then revoke them.
        Permissions[] memory requests = policy_.requestPermissions();
        _setPolicyPermissions(policy_, requests, false);

        // Get the index of the policy in the active policies array.
        uint256 idx = getPolicyIndex[policy_];

        // Get the last index of the active policies array.
        uint256 lastIdx = activePolicies.length - 1;
        if (idx != lastIdx) {
                // Get the index of the last policy in the active policy array.
                Policy lastPolicy = activePolicies[lastIdx];

                // Set the last policy at the index of the policy to deactivate.
                activePolicies[idx] = lastPolicy;

                // Set the last policy's index to the index of the policy
                // that was removed.
                getPolicyIndex[lastPolicy] = idx;
        }

        // Pop the last policy from the array.
        activePolicies.pop();

        // Delete the index of the policy being deactivated.
        delete getPolicyIndex[policy_];

        // Remove policy from array of dependents for each keycode
        // that the policy depends upon.
        _pruneFromDependents(policy_);

        // Set policy status to inactive.
        policy_.setActiveStatus(false);
}
```

We can rewrite `_pruneFromDependents` function too.
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L601-L611

if `origIndex` is the index of last item in dependents, we don't need to write any slots.

```
function _pruneFromDependents(Policy policy_) internal {
        // Retrieve all keycodes that the policy is dependent upon.
        Keycode[] memory dependencies = policy_.configureDependencies();
        uint256 depcLength = dependencies.length;

        // Loop through each keycode.
        for (uint256 i; i < depcLength; ++i) {
            // Get the stored array of policies that depend on the keycode.
            Keycode keycode = dependencies[i];
            Policy[] storage dependents = moduleDependents[keycode];

            
            // Get the index of the policy to prune in the array.
            uint256 origIndex = getDependentIndex[keycode][policy_];

            // Get the last index of the dependents array.
            uint256 lastIdx = dependents.length - 1;
            if (lastIdx != origIndex) {

                // Get the address of the last policy in the array.
                Policy lastPolicy = dependents[dependents.length - 1];

                // Overwrite the last policy with the policy being pruned.
                dependents[origIndex] = lastPolicy;
                
                // Set the index of the swapped policy to its correct spot.
                getDependentIndex[keycode][lastPolicy] = origIndex;

            }            

            // Since the last policy exists twice now in the array, pop it
            // from the end of the array.
            dependents.pop();
            
            // Delete the index of the of the pruned policy.
            delete getDependentIndex[keycode][policy_];
      }
}
```