## Centralization risks

Trusted roles:
| Name            | Description                                                                                    | Risk                                                                                                                                                                                                                                                                                                                                                            |
|-----------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SEAPORT         | The address of Seaport                                                                         | As long as the address is the Seaport contract the only risk is if Seaport has any bugs. However, if the admin of this role assigns this role to malicious address the attacker can exploit this to create fake orders (even without the signer, you can re-validate an order to lock funds in the safe)                                                        |
| CREATE_SIGNER   | An EOA that signs the orders                                                                   | Doesn’t seem to hold much power on its own                                                                                                                                                                                                                                                                                                                      |
| ADMIN_ADMIN     | Holds the power to upgrade contracts, set the fee and take protocol fees                       | Can easily steal funds from the protocol, e.g. - upgrade the escrow to a malicious contract and steal all the balance there (as long as it wasn’t frozen), whitelist a malicious extension that would help renters steal the rented tokens                                                                                                                                                                |
| GUARD_ADMIN     | Holds the power to add or remove hooks from the Guard                                          | Adding a malicious hook can help renters to steal the rented tokens, since the hook checks the tx rather than the regular tx check.                                                                                                                                                                                                                             |
| Kernel admin    | Holds the power to grant or revoke all roles (except for kernel admin and executor)            | Basically holds the power to all the roles. Therefore it bears the risk of all those roles combined.                                                                                                                                                                                                                                                            |
| Kernel executor | Holds the power to replace the modules. This would update the modules in the policies as well. | This role holds a very high power over the system, if the role holder becomes malicious they can easily steal funds from the protocol. For example - they can add a new policy that has access to all modules and through that policy steal funds from escrow and mess up the rentals (locking funds in the safe or allowing renters to steal the rented items) |

Overall it seems that most roles here hold a very high amount of power over the system and compromising them would cause loss of funds for the protocol and the users.
Therefore, all of them must be behind a timelock to allow users to withdraw their funds in case one of the roles gets compromised (with an exception of CREATE_SIGNER and escrow.skim(). The latter is protocol fees and won’t harm the users).
On top of that, given that the tokens are locked in the contracts till the end of the rental a timelock on its own wouldn’t help for orders which are longer than the timelock.
**I’d suggest adding an escape feature**, where with an agreement of both the lender and the renter the rent can end and the tokens can be extracted safely from the safe and escrow. There'd still be a risk of the admin collaborating with the renter or lender, but that's a much smaller risk.
(an alternative might be to require the rentals to be shorter than the timelock delay, but that doesn’t seem very reasonable)

#### Limiting the fee level
On top of that, it'd make sense to set a max fee, to limit the amount of fee that the owner can set, esp. considering that under current situation the fee applies retroactively.

## Pausing mechanism
Currently there's no pausing mechanism in the protocol.
However, all orders have to be signed via the CREATE_SIGNER role, that makes it possible for the admins to pause the creation of new orders by not signing orders.
Given that the intention of a pausing mechanism is to stop new activity (but allowing the completion of existing activity) this might be sufficient. The rest of the actions in the system (safe tx during rental, stopping rentals) are for rents that already begun and it wouldn't make sense to give the pauser the power to pause them.

It's worth mentioning though that given that the CERATE_SIGNER needs to constantly sign orders it would have to be a warm wallet, which put it at a higher risk of being compromised.
Therefore it might be worth to add additional pause mechanism, or make the admin of the CERATE_SIGNER role an EOA or multisig without a timelock mechanism, this way the CERATE_SIGNER role can be revoked immediately if it has been compromised.

## Default framework

### Access control
Access control to the modules is managed by the kernel.
I've verified that all external/public functions in the modules have the `permissioned` modifier which would ensure that it can only be accessed with a policy with the right permissions.

### Dual upgrade mechanism
Besides the framework's feature to update a module we can also upgrade the current modules implementation via the Admin policy.
This makes sense, since an upgrade via the kernel would mean changing to a different address which wouldn't preserve the storage of the previous address. However, it's worth keeping an eye on the fact that we have another way to upgrade the modules which might be at the hands of a different role.

## Integration with contracts not in scope
### Future code

The following types contracts are to be added to the protocol in the future, besides failing to do what they're supposed to do they bare some additional risks:

| Description                                             | Risk                                                                                                                        |
|---------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| Addresses whitelisted for a delegate call from the safe | During a delegate call we can execute anything, effectively bypass the guard.                                |
| Enabled modules                                         | Module txs aren’t checked by the guard, meaning a bug in a module can allow us to bypass the guard.                         |
| On transaction hooks                                    | Whan a hook is active the guard uses it to check the tx, a bug in a hook might allow bypassing the guard to execute any tx. |
| Stop hooks                                              | Can prevent stopping a rental, would effectively lock the rented assets and payment.                                        |
| Start hooks                                             | Can prevent starting rentals                                                                                                |

Note that bypassing the guard means you can easily steal the rented items, or even remove the guard completely.

### External libraries

|        Name        |               Integration               |                                                                            Notes                                                                            |
|:------------------:|:---------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------:|
| OpenZeppelin       | ERC20/721/1155 interfaces               | Just interfaces                                                                                                                                             |
| OpenZeppelin ECDSA | Recovering singer address at Signer.sol |                                                                                                                                                             |
| GnosisSafe         | Guard implementation                    | Requires a good understanding of how the safe and guard works.                                                                                              |
|                    | Checking the owner of a safe            | Seems rather simple                                                                                                                                         |
|                    | Deploying a safe                        |                                                                                                                                                             |
| Seaport            | `_validateOrder()` implementatoin       | There are many parameters sent from Seaport, which we process during validation. Having a wrong assumption about one of them might open the door for a bug. |


### Other contracts excluded from scope



#### Contracts with little code
The following contracts have been excluded from scope:
* `src/packages/Zone.sol`
* `src/libraries/KernelUtils.sol`
* `src/libraries/RentalUtils.sol`
* `src/proxy/Proxiable.sol`
* `src/proxy/Proxy.sol`

Most of them contain rather simple logic so the chance of having bugs in them is pretty low, still it's worth auditing those since the chance of them having bugs isn't zero.



#### Interfaces, error, events and structs
Those don't contain any logic, so the chance of them having bugs is really low.

## Critical parts of the protocol

### Rental creation and order validation

* Ensure only Seaport can call the `validateOrder()` function
* `validateOrder()` should ensure:
    * Items are sent to the right addresses
    * The CREATE_SIGNER signed the rent payload
    * Rental is registered to storage

### Guarding against rented token escape
The most difficult part here is preventing the escape of a rented token from a safe.
There might be many different ways to transfer a token and it might be difficult to try to block them all.

#### Pre approving a token
We can try to bypass the guard by approving the tokens before the rental begins, however this doesn't seem to work:
* As for ERC721 all specific approves are deleted during transfer 
    * This is according to [ERC721 specs](https://eips.ethereum.org/EIPS/eip-721#:~:text=///%20%20When%20a%20Transfer%20event%20emits%2C%20this%20also%20indicates%20that%20the%20approved%0A%20%20%20%20///%20%20address%20for%20that%20NFT%20(if%20any)%20is%20reset%20to%20none.):
        >     ///  When a Transfer event emits, this also indicates that the approved address for that NFT (if any) is reset to none.    
* ERC1155 doesn't have a function to approve a specific token
* `approveForAll()` isn't allowed at all times

#### Approving for a previous 'life' of the safe
Technically we can deploy a safe, approve tokens, destroy it and then create another safe.
However it doesn't seem to work in this case, since in order to deploy it to the same address we have to deploy it with the same parameters, which means it'd be with the same guard that would stop approving and destroying the safe.

#### Running before the guard is set
The guard is set right when the safe is created, there's no wiggle for an attacker to run any function before it's set.

#### Weird ERC721/ERC1155 tokens
This is where the biggest risk lays, tokens often take their freedom in adding new features to their own implementation. Those cool 'features' might be used to bypass the guard.
The sponsor listed this as a known issue, but it's worth noting that these might be tokens which are commonly used (e.g. crypto kitties), and the average users wouldn't expect them to cause issues when using them in the protocol.

* Alternative functions to transfer tokens
    * E.g. another parameter to add some metadata / additional instructions
* Alternative functions to approve
    * E.g. ERC1155 function to approve a specific token
* Different behavior for existing functions
    * E.g. not erasing the approval of ERC721 on transfer


### Rental stop

* Ensuring the rental exists and has ended
    * Deleting the rental from storage / Guarding against double rental stop
* Transferring payment from escrow to lender (or renter for PAY order)
* Transferring ERC721/1155 tokens back to the lender


## Reentrancy

### `onXReceived`
This seems the obvious way to achieve reentrancy here.
During rental start the tokens are transferred to the safe, the `onXReceived` functions in the fallback handler of the safe simply return and do nothing.

During rental stop the tokens are transferred back to the lender and this is where we can achieve reentrancy.
* Attempting to reenter and stop the same order again would probably revert during the check in Storage
* Attempting to reenter and stop another rental would probably not have any effect on current rental or vice versa
    * The 2 interaction that weren't done yet during the reentrancy are the settle payments and marking the rental as removed in storage. Both don't seem to be affected by another rental being stopped before they run (or them affecting the other order).
* Attempting to create another order during that time
    * It wouldn't have the same order hash, as the hash is based on the rental start timestamp as well

Despite that, it might be good not to put a reentrancy guard, or at least to move the `_reclaimRentedItems()` interaction to the bottom of the function.

### During safe tx
The safe's Guard is pretty stateless - all txs are either allowed or prohibited, therefore it doesn't seem that a reentrancy can cause any issues with current code.
As for future code - hooks, extensions and delegate calls - it's worth taking that into account when auditing the code.

## Effects of the protocol over other tokens in the safe
Safes created by the protocol can be used to store other tokens.
However the safe would still be restricted by the guard:
* Delegate calls are restricted to allowlist
* Approve for all is not allowed
* Modules are restricted to allowlist
* Non-rented ERC1155 tokens can't be transferred out of the safe if there's a similar token (same address, same token ID) that's rented.
* The protocol's modules can transfer tokens out of the safe without the owner's approval.
It's important to communicate that to the user, esp. the first effect.

## Order hash uniqueness
The `orderHash` used in the protocol is a hash of the `RentalOrder` struct which contains Seaport's `orderHash`.
This means that as long as Seaport's order hash is unique then the protocol's order hash would be unique as well (I didn't dive deep enough into Seaport to check that, but it makes sense that this is the case).
Still, it might be worth for additional security to verify during registration that the order hash wasn't registered yet.


## Push0 opcode on L2s
Some L2s might not support the `push0` opcode, given that the compiler version is greater than 0.8.20 then it might cause issues if compiled to the Shanghai evm.
I noticed the [`foundry.toml`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/foundry.toml#L8-L11) targets the `paris` evm for that reason, so as long as it's compiled for that evm this shouldn't be an issue.

### Time spent:
25 hours