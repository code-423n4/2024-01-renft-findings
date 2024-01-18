# QA Report

## Low Risk 
| Count | Title | Instances |
|:--:|:-------|:--:|
| [L-01] | Gnosis Safe disableModule's module parameter offset is incorrect. | 1 |
| [L-02] | User cannot transfer any amount of extra ERC1155 tokens from wallet if rental exists. | 1 |
| [L-03] | EIP712 hash for OrderMetadata is incorrect. | 1 |

Total Low Risk Issues: 3

## Non-Critical
| Count | Title | Instances |
|:--:|:-------|:--:|
| [NC-01] | Add a `to` address check for Gnosis Safe call guards. | 1 |

Total Non-Critical Issues: 1

## [L-01] Gnosis Safe disableModule's module parameter offset is incorrect.

### Bug Description

The offset for `gnosis_safe_disable_module_offset` is incorrectly set to `0x24`, targeting the first parameter instead of the second. But the `module` parameter is the second parameter, so the correct offset should be `0x44`.

This error could lead to administrative actions on the wrong module, allowing the safe owner to disable a module that hasn't been whitelisted. For instance, in a module list sequence of `0x01 (SENTINEL_MODULES) -> module0 -> module1 -> 0x01`, whitelisting `module0` could unintentionally permit the disabling of `module1`, which is not the intended outcome.

```solidity
uint256 constant gnosis_safe_disable_module_offset = 0x24;
```

The `disableModule()` function within Gnosis Safe `ModuleManager.sol`: 
```solidity
function disableModule(address prevModule, address module);
```

### Code Snippet

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalConstants.sol#L62
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L255-L266
- https://github.com/safe-global/safe-contracts/blob/47a3620ed937c780d26ff218b76edaaef658f7e2/contracts/base/ModuleManager.sol#L63

### Recommendation

Set `gnosis_safe_disable_module_offset` to `0x44`.

## [L-02] User cannot transfer any amount of extra ERC1155 tokens from wallet if rental exists.

### Bug description

The safe wallet's guard policy blocks the transfer of actively rented ERC1155 tokens, but it doesn't account for the quantity of ERC1155 tokens being rented. This could hinder users from transferring their ERC1155 tokens that aren't part of the rental.

For example, if a user rents 10 ERC1155 tokens and then acquires an additional 15 of the same token ID through a game reward, they should be able to transfer these extra 15 tokens. However, the current implementation prevents any transfer of the extra tokens as long as there's an active rental for the same token ID.

```solidity
function _revertSelectorOnActiveRental(
    bytes4 selector,
    address safe,
    address token,
    uint256 tokenId
) private view {
    // Check if the selector is allowed.
    if (STORE.isRentedOut(safe, token, tokenId)) {
        revert Errors.GuardPolicy_UnauthorizedSelector(selector);
    }
}
```

### Code Snippet

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L235-L243
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L126-L136

### Recommendation

For ERC1155 tokens, the ideal approach is to track the quantity being rented. The system should permit transfers as long as the wallet holds the amount of the tokens being rented.

## [L-03] EIP712 hash for OrderMetadata is incorrect.

### Bug description


The `_deriveOrderMetadataHash()` function doesn't correctly calculate the hash of `OrderMetadata`, as it omits the `orderType` and `emittedExtraData` fields. Although this isn't causing any immediate issues, as the function is used for both offchain signing and onchain validation, it's still advisable to correct this implementation for accuracy and future reliability.

```solidity
function _deriveOrderMetadataHash(
    OrderMetadata memory metadata
) internal view returns (bytes32) {
    // Create array for hooks.
    bytes32[] memory hookHashes = new bytes32[](metadata.hooks.length);

    // Iterate over each hook.
    for (uint256 i = 0; i < metadata.hooks.length; ++i) {
        // Hash the hook
        hookHashes[i] = _deriveHookHash(metadata.hooks[i]);
    }

    // Derive and return the metadata hash as specified by EIP-712.
    return
        keccak256(
            abi.encode(
                _ORDER_METADATA_TYPEHASH,
                metadata.rentDuration,
                keccak256(abi.encodePacked(hookHashes))
            )
        );
}
```

```solidity
struct OrderMetadata {
    // Type of order being created.
    OrderType orderType;
    // Duration of the rental in seconds.
    uint256 rentDuration;
    // Hooks that will act as middleware for the items in the order.
    Hook[] hooks;
    // Any extra data to be emitted upon order fulfillment.
    bytes emittedExtraData;
}
```

### Code Snippet

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L231-L239
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalStructs.sol#L50-L59

### Recommendation

Add the corresponding fields.

## [NC-01] Add a `to` address check for Gnosis Safe call guards.

### Bug description


This isn't a bug, but rather a suggestion for code enhancement. The current guard policy checks the function selector against Gnosis Safe's module and guard setting calls. Since the intention is to prevent these specific calls only when directed at the safe itself, it would be beneficial to include a check for the `to` address. This additional check could potentially allow other contracts with identical function selectors to operate without being inadvertently blocked.

### Code Snippet

- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L243
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L255
- https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L287

### Recommendation

Also add a `from == to` check while checking these calls, to prevent accidently blocking other contract function calls.
