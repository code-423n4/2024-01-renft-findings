# L-01 Missing salt paramter in EIP 712 `eip712DomainTypehash` calculation

## Lines of code

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L279

## Impact
 [EIP 712](https://eips.ethereum.org/EIPS/eip-712) specifies that a `salt` param can be used for the domain type has calculation to fully adhere to the specification whereas in this case it is not the case

## Tools Used
manual review
## Recommended Mitigation Steps
Consider `salt` when calculating `eip712DomainTypehash`

# L-02 hook item index can be out of bounds with the offer & rental item array

## Lines of code

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L488

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L218

## Impact
There is no check that the item index in the hook array is valid when adding or removing hooks which might result in tx revert when creating/stopping a rental

## Tools Used
manual review
## Recommended Mitigation Steps
add a validation check to make sure that `itemIndex <= offer.length/rental.length`

# L-03 No check for contract bytecode size during create2 deployment

## Lines of code
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L54

## Impact
deploying a contract without any bytecode will be a waste of execution overhead & gas

## Tools Used
manual review
## Recommended Mitigation Steps
add a check `if(iszero(extcodesize(deploymentAddress))) revert();`

# L-04 Avoid reacasting the `Module` instance again 

## Lines of code
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L514

## Impact
The `getModuleForKeycode` mapping returns the module instance after casting so we can avoid re-casting to save gas

## Tools Used
manual review
## Recommended Mitigation Steps
remove re-casting of the module instance

# L-05 Use `abi.encodePacked` for computing eip 712 typehashes

## Lines of code

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L373

## Impact
To completely adhere to the  [EIP 712 standard](https://eips.ethereum.org/EIPS/eip-712) `abi.encodePacked` should be used instead of `abi.encode` when comoputing `rentalOrderTypeHash`

## Tools Used
manual review
## Recommended Mitigation Steps
use `abi.encodePacked` instead of `abi.encode` when comoputing `rentalOrderTypeHash`

# L-06 Remove extra hook validation check in guard policy

## Lines of code

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L338

## Impact
`STORE.hookOnTransaction` returns whether a hook is valid or not but there is an extra check(`hook != address(0)`) to determine hook validity when a tx is triggered which is not necessary and costs extra gas


## Tools Used
manual review
## Recommended Mitigation Steps
update the if statement to `if(isActive)`