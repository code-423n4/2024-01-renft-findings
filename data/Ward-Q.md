## [L-01] Kernel: misplaced zero address check for `changeKernel`

* https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L54-L56
* https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L292-L294

Currently, the check for the `Kernel` to be a contract (also not to be the zero address), is in the current `Kernel` implementation. However, no modules and policies have the logic to ensure this as they inherit from `KernelAdapter`, which will just set the new kernel without a question. This will work well as long as the new Kernel has the similar logic to check the next Kernel’s integrity. However, if the logic is forgotten, there is no other safe guard to ensure that the next kernel is not a zero address and is a contract.
Since `Kernel` is absolutely needed for this system’s functionality, there is no possible case that the Kernel should be the zero address. Therefore, it is probably safe to add the checking logic to the `KernelAdapter`, so every module and policy will check for the next Kernel. It costs more gas since the check is done multiple times, but still arguably it is worth the cost, since Kernel is core part of the system and it will not updated very often.

```solidity
// KernelAdapter::changeKernel
54     function changeKernel(Kernel newKernel_) external onlyKernel {
55         kernel = newKernel_;
56     }

// Kernel::executeAction
292     } else if (action_ == Actions.MigrateKernel) {
293         ensureContract(target_);
294         _migrateKernel(Kernel(target_));
```

## [N-01] Kernel: missing zero address check for `executor` and `admin`

* https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L295-L298

The `executor` and `admin` are not checked for the zero address when set by the `Kernel::executeAction`.

```solidity
// Kernel::executeAction
295     } else if (action_ == Actions.ChangeExecutor) {
296         executor = target_;
297     } else if (action_ == Actions.ChangeAdmin) {
298         admin = target_;