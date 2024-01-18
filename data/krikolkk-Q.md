# L-01 Enabling and disabling modules can lead to an undefined behavior

## Impact

The [_checkTransaction](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195) function checks whether a contract is whitelisted for a rental safe to add as a module or remove from modules. However, the function checks the same condition in both cases, which can lead to undefined behavior.

1. Assume that the Stop policy is being upgraded via `enableModule`, so the new contract will be whitelisted
2. Bob enables this new `Stop` policy in his safe and rents an NFT
3. Since the module is whitelisted, Bob can call `disableModule`, causing the `Stop` policy to be unable to call `reclaimRentalAssets`, essentially locking the NFTs in his safe

We used the Stop policy in this example as it is currently the only policy manipulating the NFTs. This issue is relevant to any module that can manipulate the safe’s assets.

## Recommended mitigation steps:
Consider differentiating between whitelisting modules for enabling and whitelisting modules for disabling, or add some form of check to check whether a safe is being used for any rentals and only then allow disabling a module.

# N-01 Code repetition

The function [stopRentBatch](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L313) contains the same instructions as [stopRent](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265). The only difference is that the stopRentBatch function does it in a loop. Consider joining the repetitive parts of code together to save on contract size and follow the best coding practices.

# N-02 Code repetition

The function [updateHookPath](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L294) adds a hook to an NFT contract, that will be executed throughout the rental lifecycle. However, the address `to` is not enforced to implement the ERC721/ERC1155 interface. This breaks the invariant “Hooks can only be specified for ERC721 and ERC1155 items” specified in the [main invariants](https://github.com/code-423n4/2024-01-renft/tree/main?tab=readme-ov-file#main-invariants).

