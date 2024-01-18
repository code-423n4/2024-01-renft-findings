# Low

## [L-01] Hash collision in `_deriveOrderMetadataHash()` 
[Signer.sol Line 218-239](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L218-L239)

**Issue Description:**

The function `_deriveOrderMetadataHash()` in `Signer.sol` is crucial for verifying the consistency of metadata used in rental orders. It ensures that the metadata provided during order fulfillment matches the metadata initially set by the order's creator.

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

	//@audit: This is missing the emittedExtraData and OrderType parameter of OrderMetadata struct

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

The OrderMetadata struct looks as follows:

```solidity
struct OrderMetadata {
    // Type of order being created.
    OrderType orderType; <-- Missing
    // Duration of the rental in seconds.
    uint256 rentDuration;
    // Hooks that will act as middleware for the items in the order.
    Hook[] hooks;
    // Any extra data to be emitted upon order fulfillment.
    bytes emittedExtraData; <-- Missing
}
```

The `OrderMetadata` struct includes `orderType`, `rentDuration`, `hooks`, and `emittedExtraData`. However, the current implementation of `_deriveOrderMetadataHash()` omits the `orderType` and `emittedExtraData` parameters in its hashing process.

This omission raises the possibility of hash collisions for orders with identical `rentDuration` and `hooks` but different `orderType` and `emittedExtraData`. Consequently, the `_isValidOrderMetadata()` function may erroneously validate non-identical `OrderMetadata` structs.

**Recommended Mitigation Steps:**

To rectify this vulnerability, it is essential to modify the hashing function to incorporate the missing `orderType` and `emittedExtraData` parameters. This enhancement ensures a more comprehensive and collision-resistant hash generation. The revised hashing function should be as follows:

```solidity
	return
		keccak256(
			abi.encode(
				_ORDER_METADATA_TYPEHASH,
				metadata.orderType
				metadata.rentDuration,
				keccak256(abi.encodePacked(hookHashes)),
				metadata.emittedExtraData
			)
		);
```

## [L-02] Blacklisted Malicious modules that are enabled can not be removed
[Guard.sol Line 255-266](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L255-L266)

**Issue Description:**

n the reNFT protocol, admins have the capability to whitelist and blacklist modules, which serves as a security measure by controlling the modules that can be enabled or disabled on a Gnosis safe.

However, a significant issue arises if a previously trusted and whitelisted module becomes vulnerable, broken or malicious over time. While admins can remove such a module from the whitelist, thus preventing its addition to new safes, this action inadvertently prevents users who have already enabled the module in their safes from disabling it as the Guard policy only allows calls to `disableModule()` for whitelisted modules. Consequently, users are left with no option but to deploy a new safe, potentially abandoning their NFTs in the compromised one.

**Recommended Mitigation Steps:**

To resolve this problem, the protocol should transition from a whitelist-based approach to a blacklist-based one for module management. This change would enable users to disable any module unless it is explicitly blacklisted. The blacklist, managed by admins, should include essential reNFT modules that are integral to the protocol's functionality, like the Stop module. By implementing this approach, the protocol can maintain its security measure to prevent disabling of critical reNFT modules, while simultaneously offering users the flexibility to remove modules that have become risky or compromised. This shift to a blacklist system provides a more dynamic and responsive security model, adapting to the evolving nature of module safety and integrity.

## [L-03] - Hooks can be specified for non ERC721 / ERC1155 items

**Issue Description:**

The reNFT contest description includes main invariants of the protocol which should not be broken. One of those invariants states that "Hooks can only be specified for ERC721 and ERC1155 items". This is incorrect as the only requirement that is placed upon hooks is that they can only be specified for contracts, as one can see in the `updateHookPath()` function below:

```solidity
function updateHookPath(address to, address hook) external onlyByProxy permissioned {
	// Require that the `to` address is a contract.
	if (to.code.length == 0) revert Errors.StorageModule_NotContract(to);

	// Require that the `hook` address is a contract.
	//@audit: Would not work if called during hook constructor
	if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);

	// Point the `to` address to the `hook` address.
	_contractToHook[to] = hook;
}
```

**Recommended Mitigation Steps:**

Achieving the functionality of only allowing an address to be of a certain ERC is complicated. One approach would be to use the ERC165 functionality to verify if the contract implements ERC1155 or ERC721. Nevertheless not all of these tokens implement ERC165, so it could potentially not be possible to define a hook for a contract that adheres to the ERC721/ERC1155 interface, but does not implement ERC165.