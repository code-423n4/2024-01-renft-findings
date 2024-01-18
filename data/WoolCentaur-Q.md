## Transfer of ownership through `Actions.ChangeExecutor` should be a two step procedure.

**Note: this two step procedure on critical functions is in the automated findings report, but missed this issue.**

### Description
In the `Kernel.sol` contract, the `executeAction()` function:
```
function executeAction(Actions action_, address target_) external onlyExecutor {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L277-L302

can only be called by the current executor, per the `onlyExecutor` modifier,
and has the ability the change the executor to any address:
```
else if (action_ == Actions.ChangeExecutor) {
            executor = target_;
```

As provided in the audit details in the Default Framework README:
"In the Kernel, the executor is the address with the permission to execute instructions—you can think of the executor as the “owner” of the Kernel contract."

The executor has the permission to install and upgrade modules, activate and deactivate policies, migrate the kernel contract, and change both the admin and executor.  Consequently, if `Actions.ChangeExecutor` is called and is by chance set to an invalid target address, reNFT will lose control over the kernel contract.

### Tools Used
Manual Analysis

### Recommendation
Consider implementing a two step process where the `Actions.ChangeExecutor` first nominates an account and second, the nominated account calls an `acceptOwnership()` function for the transfer of executor to fully succeed. This ensures the nominated account is a valid and active account.