## L1 - `Factory.deployRentalSafe()` can be front-run to revert safe creation

Consider the following scenario:
* A user sends out a tx to call `Factory.deployRentalSafe()`
* The attacker sees that and front runs it, calling `SafeProxyFactory.createProxyWithNonce()` directly with the same parameters
* The user's tx would revert because the safe already exists

The user can mitigate that by first creating a safe with a different set or order of owners, or a different threshold, and then creating the safe they wished to create (the attacker can still keep front-running those txs though).

A way to mitigate that (prevent front running) is to deploy a fork of `SafeProxyFactory` with access control - allowing only the protocol's Factory to create new safes.


## L2 - initializer can be front run
Both modules are using a proxy with initializers, those initializers can be front run during deployment.
As mitigation - ensure that initialization runs at the same tx with deployment.

## L3 - Renter can steal CryptoKitties token using `transfer()`
`CryptoKitties` uses a non-standard ERC721 interface, this means that in case it's rented it can be stolen by the renter since the Guard can't stop it.
The collection is [pretty popular](https://opensea.io/collection/cryptokitties), and it can be [traded on Seaport](https://etherscan.io/tx/0x712bedbdbfeacb4bf93d84849d25e48159effd838ee114bab63d43176c08746e) using Conduit.
This might be classified as a known issue, but given that `CryptoKitties` is commonly - it's worth noting that.

## L4 - On transaction hooks aren’t controlled by the lender
Contrary to the docs [which state](https://github.com/code-423n4/2024-01-renft/blob/main/docs/hooks.md#:~:text=When%20signing%20a%20rental%20order%2C%20the%20lender%20can%20decide%20to%20include%20an%20array%20of%20Hook%20structs%20along%20with%20it.%20These%20are%20bespoke%20restrictions%20or%20added%20functionality%20that%20can%20be%20applied%20to%20the%20rented%20token%20within%20the%20wallet.) that the hooks are controlled by the lender - in reality all active on-transaction hooks run regardless of what the lender has specified in the order.


## L5 - impossible to disable modules that has been removed from whitelist
Consider the following scenario:
* Module has been whitelisted
* Some safes add it
* Module has been removed from whitelist
* Safes can’t disable this module now

Mitigation - either make a separate allowlist for disabling, or allow disabling all modules but the Stop module

## L6 - limit the rental to a reasonable time, to prevent creating for an unreasonable time by mistake
Currently it seems that the protocol allows rental for thousands of years, that doesn’t make much sense and if a user specifies that by mistake that might lock their tokens forever.

## L7 - if stop module is whitelisted it can be disabled by the safe
If stop module is whitelisted by mistake - renters can just disable it and prevent rental stop.
As mitigation, consider adding a check during whitelisting to prevent whitelisting it.

## L8 - Can’t update the status of a self destructed hook

The protocol doesn't allow updating the status of hooks with no code.
The issue is that if a hook is self destructed for any reason the admin can't update its status any more.

[code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L318-L319)
```solidity
        if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);
```

As mitigation - if the hook doesn't have any code allow only disabling it (setting the status to zero).

## L9 - if stop policy is replaced it’d brick the rental stop for older safes
The stop policy can be replaced via the kernel, if the old stop policy is disabled it'd be impossible to stop the rentals - since only the old stop policy has access to the safe.

As mitigation - either keep the old stop policy active, or add some migration mechanism (allowing the old stop policy to add the new stop policy to the safe as a module. This has to be done before deploying the current stop policy)

## R1 - `Accumulator._insert()` might result in memory collision in future code
This function expands the size of the `rentalAssets` memory variable, assuming there's no memory that's currently used afterwards.
This is true under current code, but worth keeping an eye on it for future code changes or upgrades - if there's another memory variable after `rentalAssets` when this function is called it'd override it, causing severe issues.

[Code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L81-L83)
```solidity
            // Update the free memory pointer so that memory is safe
            // once we stop doing dynamic memory array inserts
            mstore(0x40, add(newItemPosition, 0x40))
```

## R2 - impossible to remove a hook from a specific contract
While it’s possible to disable a hook, that would disable it for all contracts that are using it. If we want to enable it on one target and remove it from another target that’s impossible, because we can’t set it back to the zero address (since the zero address doesn’t have any code).
As mitigation, allow setting it to the zero address.

[code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L298-L300)
```solidity
        // Require that the `hook` address is a contract.
        if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);

```
