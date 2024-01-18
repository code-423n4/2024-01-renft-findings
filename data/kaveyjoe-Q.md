## [Q- 01 ] No Module uses the INIT() function which is enstated in Kernel.sol in the Module class.


In the abstract Module class within Kernel.sol, the INIT() function is declared without any implementation present, with the intention evidently being to implement the respective INIT codes within the Modules themselves. However, none of the present Modules are overriding the INIT function and so, whenever the kernel is installing a new module and the _newModule.INIT() function is being called, no initialisation is happening on the modules.


## Proof of concept 

None of the modules are using the INIT function which has been created in Kernel.sol's Module class:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L106

Used here:
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L406

And
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L375


Nothing gets initialised for any existing module through these initialisations, so in current context it is using deployment gas unnecessarily.



## [Q-02]executor can become also admin




Changing critical addresses such as ownership should be a two-step process where the first transaction (from the old/current address) registers the new address (i.e. grants ownership) and the second transaction (from the new address) replaces the old address with the new one. This gives an opportunity to recover from incorrect addresses mistakenly used in the first step. If not, contract functionality might become inaccessible.


## Proof of concept 
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L298


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L296


## [Q-03]- Missing events for critical changes in system parameters


Change of critical system parameters should come with an event emission to allow for monitoring of potentially dangerous or malicious changes.

## Proof of conept 

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L55


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L193





## [Q-04] - Zero address check missing



https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L54


 ## observed that new kernel can be set as zero address which will prohibit all kernel activities

Recommendation

Do not allow zero address while setting kernel

## [Q-05] - Not verified input

At the following function you should verify the parameters that are being assigned to a state variable.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L277


## [Q-06]Events not emmited on important state changes

Emmiting events is recommended each time when a state variable's value is being changed or just some critical event for the contract has occurred. It also helps off-chain monitoring of the contract's state.




https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L148

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L166


## [Q-07]Requirement violation.


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L431


A requirement was violated in a nested call and the call was reverted as a result. Make sure valid inputs are provided to the nested call (for instance, via passed arguments).
Reference: https://swcregistry.io/docs/SWC-123


## [Q-08] - _activatePolicy is non CEI conformant

The function _activatePolicy will perform an external call to policy_.configureDependencies() and then it will change storage.


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L430C9-L450C6



## [Q-09] - public functions not called by the contract should be declared external instead


Contracts are allowed to override their parents' functions and change the visibility from external to public.


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L333C1-L345C6

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L310C5-L325C6


## [Q-10] - Natspec is incomplete


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L54

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L277

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L310
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L333


