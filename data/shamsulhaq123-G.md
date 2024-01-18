## Gas Optimization

## Summary

|No|Issue|instence|
|--|-----|--------|
|[G-01]|Use nested if statements instead of &&|6|
|[G-02]|Use hardcode address instead address(this)|12|
|[G-03]|Use function instead of modifiers|4| 
|[G-04]|Use do while loops instead of for loops|29|
|[G-05]|+= costs more gas than = + for state variables|2|
|[G-06]|Use assembly for loops|29|
|[G-07]|Caching global variables is more expensive than using the actual variable (use msg.sender instead of caching it)|12|
|[G-08]|Remove or replace unused state variables|7|
|[G-09]|Use shift Right instead of division if possible to save gas|2|
|[G-10]|Use shift Left instead of multiplication if possible to save gas|1|
|[G-11]|Avoid transferring amounts of zero in order to save gas|3|
|[G-12]|Using mappings instead of arrays to avoid length checks save gas|1|
|[G-13]|Change public function visibility to external|5|
|[G-14]|Should use arguments instead of state variable |10|
|[G-15]Gas saving is achieved by removing the delete keyword (~60k)|4|
|[G-16]|Unnecessary look up in if condition|7|
|[G-17]|Using storage instead of memory for structs/arrays saves gas|22|
|[G-18]|Before transfer of  some functions, we should check some variables for possible gas save|3|

## [G-01] Use nested if statements instead of &&
If the if statement has a logical AND and is not followed by an else statement, it can be replaced with 2 if statements.

```solidity

file: modules/PaymentEscrow.sol

115 if (!success || (data.length != 0 && !abi.decode(data, (bool)))) 

255    if (orderType.isPayOrder() && !isRentalOver)

268  (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L115
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L255
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L268

```solidity

file: policies/Guard.sol

324  if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) 

338  if (hook != address(0) && isActive)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L324
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L338

```solidity

file: policies/Stop.sol

148  if (!isLender && (!hasExpired)) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L148

## [G-02] Use hardcode address instead address(this)
Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address.

```solidity

file: modules/PaymentEscrow.sol

402   uint256 trueBalance = IERC20(token).balanceOf(address(this));
``` 
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L402

```solidity

file: packages/Reclaimer.sol

23   original = address(this);

33  IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier);

44 address(this),

73  if (address(this) == original) 

80  if (address(this) != rentalOrder.rentalWallet)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L23
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L44
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L73
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L80

```solidity

file: packages/Signer.sol

285     address(this)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L285

```solidity

file: policies/Factory.sol

124   ISafe(address(this)).enableModule(_stopPolicy);

127   ISafe(address(this)).setGuard(_guardPolicy);

163   address(this),
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L127
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L163

```solidity

file: policies/Stop.sol

170    address(this),
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L170

```solidity

file: Create2Deployer.sol

90   abi.encodePacked(create2_ff, address(this), salt, keccak256(initCode))
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L90


## [G-03] Use function instead of modifiers 

```solidity

file: Kernel.sol

40   modifier onlyKernel() {
        if (msg.sender != address(kernel))
            revert Errors.KernelAdapter_OnlyKernel(msg.sender);
        _;
    }

77    modifier permissioned() {
        if (!kernel.modulePermissions(KEYCODE(), Policy(msg.sender), msg.sig)) {
            revert Errors.Module_PolicyNotAuthorized(msg.sender);
        }

130  modifier onlyRole(bytes32 role_) {
        Role role = toRole(role_);
        if (!kernel.hasRole(msg.sender, role)) {
            revert Errors.Policy_OnlyRole(role);
        }        
254  modifier onlyExecutor() {
        if (msg.sender != executor) revert Errors.Kernel_OnlyExecutor(msg.sender);
        _;
    }

    /**
     * @dev modifier which only allows calls by an admin address.
     */
262    modifier onlyAdmin() {
        if (msg.sender != admin) revert Errors.Kernel_OnlyAdmin(msg.sender);
        _;
    }        
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L40
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L77
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L130
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L254-L262

## [G-04] Use do while loops instead of for loops

A do while loop will cost less gas since the condition is not being checked for the first iteration.

```solidity

file: modules/PaymentEscrow.sol

231       for (uint256 i = 0; i < items.length; ++i) 

341   for (uint256 i = 0; i < orders.length; ++i) 
``` 
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L231
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L341

```solidity

file: modules/Storage.sol

197   for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) 

229     for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) 

249    for (uint256 i = 0; i < orderHashes.length; ++i) 

260     for (uint256 i = 0; i < rentalAssetUpdates.length; ++i)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L197
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L229
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L249
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L260

```solidity

file: packages/Accumulator.sol

120   for (uint256 i = 0; i < rentalAssetUpdateLength; ++i)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L120

```solidity

file: packages/Reclaimer.sol

90  for (uint256 i = 0; i < itemCount; ++i) 


```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L90

```solidity

file: packages/Signer.sol

170    for (uint256 i = 0; i < order.items.length; ++i) 

176  for (uint256 i = 0; i < order.hooks.length; ++i)

225  for (uint256 i = 0; i < metadata.hooks.length; ++i) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L170
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L176
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L225

```solidity

file: policies/Create.sol

209   for (uint256 i; i < offers.length; ++i) 

261  for (uint256 i; i < offers.length; ++i) 

337 for (uint256 i; i < considerations.length; ++i)

375    for (uint256 i; i < considerations.length; ++i) 

475   for (uint256 i = 0; i < hooks.length; ++i) 

567    for (uint256 i; i < items.length; ++i)

599 for (uint256 i = 0; i < items.length; ++i)

695   for (uint256 i = 0; i < executions.length; ++i) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L209
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L261
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L337
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L375
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L475
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L567
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L599
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L695

```solidity

file: policies/Stop.sol

205    for (uint256 i = 0; i < hooks.length; ++i)

276  for (uint256 i; i < order.items.length; ++i)

324     for (uint256 i = 0; i < orders.length; ++i) 

333  for (uint256 j = 0; j < orders[i].items.length; ++j) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L276
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L324
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L333

```solidity

file: Kernel.sol

438   for (uint256 i; i < depLength; ++i) 

512   for (uint256 i; i < keycodeLen; ++i) 

521  for (uint256 j; j < policiesLen; ++j) 

546  for (uint256 i; i < depLength; ++i)

566  for (uint256 i = 0; i < reqLength; ++i) 

592  for (uint256 i; i < depcLength; ++i)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L438
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L512
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L521
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L546
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L566
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L592

## [G-05] += costs more gas than = + for state variables
use = + or = - instead to save gas

```solidity

file: modules/PaymentEscrow.sol

306  balanceOf[token] += amount;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L306

```solidity

file: modules/Storage.sol

201    rentedAssets[asset.rentalId] += asset.amount;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L201

## [G-06] Use assembly for loops
In the following instances, assembly is used for more gas efficient loops.

```solidity

file: modules/PaymentEscrow.sol

231       for (uint256 i = 0; i < items.length; ++i) 

341   for (uint256 i = 0; i < orders.length; ++i) 
``` 
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L231
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L341

```solidity

file: modules/Storage.sol

197   for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) 

229     for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) 

249    for (uint256 i = 0; i < orderHashes.length; ++i) 

260     for (uint256 i = 0; i < rentalAssetUpdates.length; ++i)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L197
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L229
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L249
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L260

```solidity

file: packages/Accumulator.sol

120   for (uint256 i = 0; i < rentalAssetUpdateLength; ++i)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L120

```solidity

file: packages/Reclaimer.sol

90  for (uint256 i = 0; i < itemCount; ++i) 


```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L90

```solidity

file: packages/Signer.sol

170    for (uint256 i = 0; i < order.items.length; ++i) 

176  for (uint256 i = 0; i < order.hooks.length; ++i)

225  for (uint256 i = 0; i < metadata.hooks.length; ++i) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L170
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L176
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L225

```solidity

file: policies/Create.sol

209   for (uint256 i; i < offers.length; ++i) 

261  for (uint256 i; i < offers.length; ++i) 

337 for (uint256 i; i < considerations.length; ++i)

375    for (uint256 i; i < considerations.length; ++i) 

475   for (uint256 i = 0; i < hooks.length; ++i) 

567    for (uint256 i; i < items.length; ++i)

599 for (uint256 i = 0; i < items.length; ++i)

695   for (uint256 i = 0; i < executions.length; ++i) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L209
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L261
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L337
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L375
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L475
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L567
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L599
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L695

```solidity

file: policies/Stop.sol

205    for (uint256 i = 0; i < hooks.length; ++i)

276  for (uint256 i; i < order.items.length; ++i)

324     for (uint256 i = 0; i < orders.length; ++i) 

333  for (uint256 j = 0; j < orders[i].items.length; ++j) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L276
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L324
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L333

```solidity

file: Kernel.sol

438   for (uint256 i; i < depLength; ++i) 

512   for (uint256 i; i < keycodeLen; ++i) 

521  for (uint256 j; j < policiesLen; ++j) 

546  for (uint256 i; i < depLength; ++i)

566  for (uint256 i = 0; i < reqLength; ++i) 

592  for (uint256 i; i < depcLength; ++i)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L438
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L512
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L521
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L546
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L566
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L592


## [G‑07] Caching global variables is more expensive than using the actual variable (use msg.sender instead of caching it)

```solidity

file: policies/Guard.sol

339      _forwardToHook(hook, msg.sender, to, value, data);
        }
        // If no hook exists, use basic tx check.
        else {
343            _checkTransaction(msg.sender, to, data);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L339-L343

```solidity

file: policies/Stop.sol

135     bool isLender = expectedLender == msg.sender;

141   revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);

149  revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);

305  _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);

356             _emitRentalOrderStopped(orderHashes[i], msg.sender);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L135
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L141
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L149
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L305
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L356

```solidity

file: Create2Deployer.sol

37 if (address(bytes20(salt)) != msg.sender) {
            revert Errors.Create2Deployer_UnauthorizedSender(msg.sender, salt);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L37-L38

```solidity

file: Kernel.sol

41  if (msg.sender != address(kernel))
42        revert Errors.KernelAdapter_OnlyKernel(msg.sender);

78      if (!kernel.modulePermissions(KEYCODE(), Policy(msg.sender), msg.sig)) {
            revert Errors.Module_PolicyNotAuthorized(msg.sender);

132   if (!kernel.hasRole(msg.sender, role)) 

255   if (msg.sender != executor) revert Errors.Kernel_OnlyExecutor(msg.sender);

263  if (msg.sender != admin) revert Errors.Kernel_OnlyAdmin(msg.sender);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L41-L42
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L78-L79
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L132
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L255
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L263

## [G-08] Remove or replace unused state variables

```solidity

file: modules/PaymentEscrow.sol

25  mapping(address token => uint256 amount) public balanceOf;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L25

```solidity

file: modules/Storage.sol#

20    mapping(bytes32 orderHash => bool isActive) public orders;

    // Points an item ID to its number of actively rented tokens. This is used to
    // determine if an item is actively rented within the protocol. For ERC721, this
    // value will always be 1 when actively rented. Any inactive rentals will have a
    // value of 0.
26    mapping(RentalId itemId => uint256 amount) public rentedAssets;

33  mapping(address safe => uint256 nonce) public deployedSafes;

44   mapping(address to => address hook) internal _contractToHook;

    // Mapping of a bitmap which denotes the hook functions that are enabled.
47    mapping(address hook => uint8 enabled) public hookStatus;

55  mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;

    // Allows for the safe registration of extensions that can be enabled on a safe.
58    mapping(address extension => bool isWhitelisted) public whitelistedExtensions;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L20-L26
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L33
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L44
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L55-L58

```solidity

file: Create2Deployer.sol

16   mapping(address => bool) public deployed;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L16

```solidity

file: Kernel.sol

213    mapping(Keycode => Module) public getModuleForKeycode; // get contract for module keycode.
    mapping(Module => Keycode) public getKeycodeForModule; // get module keycode for contract.

    // Module dependents data. Manages module dependencies for policies.
    mapping(Keycode => Policy[]) public moduleDependents;
    mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

    // Module <> Policy Permissions. Keycode -> Policy -> Function Selector -> Permission.
    mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
        public modulePermissions; // for policy addr, check if they have permission to call the function in the module.

    // List of all active policies.
    Policy[] public activePolicies;
    mapping(Policy => uint256) public getPolicyIndex;

    // Policy roles data.
    mapping(address => mapping(Role => bool)) public hasRole;
230    mapping(Role => bool) public isRole;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L213-L230

## [G-09] Use shift Right instead of division if possible to save gas

```solidity

file: modules/PaymentEscrow.sol

90    return (amount * fee) / 10000;

142   renterAmount = ((numerator / totalTime) + 500) / 1000;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L90
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L142

## [G-10] Use shift Left instead of multiplication if possible to save gas

```solidity

file: modules/PaymentEscrow.sol

138  uint256 numerator = (amount * elapsedTime) * 1000;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L138

## [G-11] Avoid transferring amounts of zero in order to save gas
Skipping the external call when nothing will be transferred, will save at least **100 gas**

```solidity

file: modules/PaymentEscrow.sol

175  _safeTransfer(token, lender, lenderAmount);

        // Send the renter portion of the payment.
178      _safeTransfer(token, renter, renterAmount);

201  _safeTransfer(token, settleToAddress, amount);

408   _safeTransfer(token, to, skimmedBalance);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L175-L178
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L201
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L408

## [G-12] Using mappings instead of arrays to avoid length checks save gas
Just by using a mapping, we get a gas saving of 2102 gas. When you read the value of an index of an array, solidity adds bytecode that checks that you are reading from a valid index 

```solidity

file: Kernel.sol#

217  mapping(Keycode => Policy[]) public moduleDependents;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L217

## [G-13] Change public function visibility to external
Since the following public functions are not called from within the contract, their visibility can be made external for gas optimization.

```solidity

file: modules/PaymentEscrow.sol

75  function KEYCODE() public pure override returns (Keycode) {
        return Keycode.wrap("ESCRW");
    }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L75

```solidity

file: Create2Deployer.sol

84   function getCreate2Address(
        bytes32 salt,
        bytes memory initCode
    ) public view returns (address) 

107 function generateSaltWithSender(
        address sender,
        bytes12 data
    ) public pure returns (bytes32 salt) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L84
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L107

```solidity

file: Kernel.sol

310  function grantRole(Role role_, address addr_) public onlyAdmin 

333   function revokeRole(Role role_, address addr_) public onlyAdmin 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L310
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L333

## [G-14] Should use arguments instead of state variable 

state variables should not used in emit  ,  This will save near 97 gas   

```solidity

file: modules/PaymentEscrow.sol

411     emit Events.FeeTaken(token, skimmedBalance);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L411

```solidity

file: policies/Create.sol

171  emit Events.RentalOrderStarted
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L171

```solidity

file: policies/Factory.sol#

192  emit Events.RentalSafeDeployment(safe, owners, threshold);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L192

```solidity

file: policies/Stop.sol

113  emit Events.RentalOrderStopped(seaportOrderHash, stopper);

305 _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);

356  _emitRentalOrderStopped(orderHashes[i], msg.sender);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L113
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L305
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L356

```solidity

file: Kernel.sol

301   emit Events.ActionExecuted(action_, target_);

324  emit Events.RoleGranted(role_, addr_);

344  emit Events.RoleRevoked(role_, addr_);

571  emit Events.PermissionsUpdated(
                request.keycode,
                policy_,
                request.funcSelector,
                grant_
            );

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L301
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L324
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L344
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L571



## [G-15] Gas saving is achieved by removing the delete keyword (~60k)
30k gas savings were made by removing the delete keyword. The reason for using the delete keyword here is to reset the struct values (set to default value) in every operation. However, the struct values do not need to be zero each time the function is run. Therefore, the delete keyword is unnecessary. If it is removed, around 30k gas savings will be achieved.

```solidity

file: modules/Storage.sol

225  delete orders[orderHash];

255 delete orders[orderHashes[i]];
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L225
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L255

```solidity

file: Kernel.sol

482  delete getPolicyIndex[policy_];

614   delete getDependentIndex[keycode][policy_];
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L482
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L614

## [G-16] Unnecessary look up in if condition
If the || condition isn’t required, the second condition will have been looked up unnecessarily.

```solidity

file:modules/PaymentEscrow.sol

115   if (!success || (data.length != 0 && !abi.decode(data, (bool)))) 

267  else if (
                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L115
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L267

```solidity

file: policies/Create.sol

311  if (totalRentals == 0 || totalPayments == 0) 

396  if (totalRentals == 0 || totalPayments == 0)

557 payload.metadata.orderType.isBaseOrder() ||
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L311
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L396
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L557

```solidity

file: policies/Factory.sol

143  if (threshold == 0 || threshold > owners.length) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L143

```solidity

file: Kernel.sol

392 if (address(oldModule) == address(0) || oldModule == newModule_) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L392

## [G-17] Using storage instead of memory for structs/arrays saves gas

```solidity

file: modules/PaymentEscrow.sol

233  Item memory item = items[i];
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L233


```solidity

file: modules/Storage.sol

191   RentalAssetUpdate[] memory rentalAssetUpdates


```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L191

```solidity

file: packages/Accumulator.sol

96  function _convertToStatic(
        bytes memory rentalAssetUpdates
    ) internal pure returns (RentalAssetUpdate[] memory updates) 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L96

```solidity

file: packages/Reclaimer.sol#

91  Item memory item = rentalOrder.items[i];
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L91

```solidity

file: packages/Signer.sol

166  bytes32[] memory itemHashes = new bytes32[](order.items.length);
167  bytes32[] memory hookHashes = new bytes32[](order.hooks.length);

222   bytes32[] memory hookHashes = new bytes32[](metadata.hooks.length);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L166-L167
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L222

```solidity

file: policies/Admin.sol

44 returns (Keycode[] memory dependencies)

69  returns (Permissions[] memory requests)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L44
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L69

```solidity

file: policies/Create.sol

692  ReceivedItem[] memory executions,

696  ReceivedItem memory execution = executions[i];
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L692
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L696

```solidity

file: policies/Factory.sol

77 returns (Keycode[] memory dependencies)

99   returns (Permissions[] memory requests)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L77
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L99

```solidity

file: policies/Guard.sol

67 returns (Keycode[] memory dependencies)

89  returns (Permissions[] memory requests)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L67
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L89

```solidity

file: policies/Stop.sol

67  returns (Keycode[] memory dependencies)

92   returns (Permissions[] memory requests)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L67
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L92

```solidity

file: Kernel.sol

424   Permissions[] memory requests = policy_.requestPermissions();

434  Keycode[] memory dependencies = policy_.configureDependencies();

462   Permissions[] memory requests = policy_.requestPermissions();

542  Policy[] memory dependents = moduleDependents[keycode_];

562   Permissions[] memory requests_,

588  Keycode[] memory dependencies = policy_.configureDependencies();
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L424
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L434
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L462
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L542
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L562
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L588


## [G-18] Before transfer of  some functions, we should check some variables for possible gas save

Before transfer, we should check for amount being 0 so the function doesn't run when its not gonna do anything:

``solidity

file: modules/PaymentEscrow.sol

175  _safeTransfer(token, lender, lenderAmount);

        // Send the renter portion of the payment.
178      _safeTransfer(token, renter, renterAmount);

201  _safeTransfer(token, settleToAddress, amount);

408   _safeTransfer(token, to, skimmedBalance);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L175-L178
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L201
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L408

