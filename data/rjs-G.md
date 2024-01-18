> Please note: this report has been reviewed to ensure it *does not* contain automated findings from the bot race winner or 4naly3er

# Summary

## Gas Optimizations
| # | Title | Instances |
| - | - | - |
| G-01 | [`do`-`while` is cheaper than `for`-loops when the initial check can be skipped](#g-01-do-while-is-cheaper-than-for-loops-when-the-initial-check-can-be-skipped) | 29 |
| G-02 | [The result of function calls should be cached rather than re-calling the function](#g-02-the-result-of-function-calls-should-be-cached-rather-than-re-calling-the-function) | 50 |
| G-03 | [`abi.encode()` may be less efficient than `abi.encodePacked()`](#g-03-abiencode-may-be-less-efficient-than-abiencodepacked) | 9 |
| G-04 | [Use `calldata` instead of `memory` for function arguments that are read only](#g-04-use-calldata-instead-of-memory-for-function-arguments-that-are-read-only) | 13 |
| G-05 | [Inline `internal` functions that are only called once](#g-05-inline-internal-functions-that-are-only-called-once) | 30 |
| G-06 | [Use named return values](#g-06-use-named-return-values) | 23 |
| G-07 | [Use a constant for literal `keccak256()` hashes](#g-07-use-a-constant-for-literal-keccak256-hashes) | 1 |
| G-08 | [Refactor modifiers to call a local function](#g-08-refactor-modifiers-to-call-a-local-function) | 5 |
| G-09 | [Inline `modifier`s that are only used once, to save gas](#g-09-inline-modifiers-that-are-only-used-once-to-save-gas) | 6 |
| G-10 | [`<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables](#g-10-x--y-costs-more-gas-than-x--x--y-for-state-variables) | 5 |
| G-11 | [Use Solady library where possible to save gas](#g-11-use-solady-library-where-possible-to-save-gas) | 3 |
| G-12 | [Cache multiple accesses of mapping/array values](#g-12-cache-multiple-accesses-of-mappingarray-values) | 72 |
| G-13 | [Cache state variables accessed multiple times in the same function](#g-13-cache-state-variables-accessed-multiple-times-in-the-same-function) | 33 |
| G-14 | [Avoid updating storage when the value hasn't changed](#g-14-avoid-updating-storage-when-the-value-hasnt-changed) | 6 |
| G-15 | [Using `storage` instead of `memory` for structs/arrays saves gas](#g-15-using-storage-instead-of-memory-for-structsarrays-saves-gas) | 71 |
| G-16 | [Use assembly to write storage values](#g-16-use-assembly-to-write-storage-values) | 56 |
| G-17 | [Redundant type conversion](#g-17-redundant-type-conversion) | 1 |
| G-18 | [Not using the named return variable is confusing and can waste gas](#g-18-not-using-the-named-return-variable-is-confusing-and-can-waste-gas) | 8 |
| G-19 | [Mutable state variables explicitly initialized to zero consume gas unnecessarily](#g-19-mutable-state-variables-explicitly-initialized-to-zero-consume-gas-unnecessarily) | 18 |

# Gas Optimizations

## [G-01] `do`-`while` is cheaper than `for`-loops when the initial check can be skipped

A `do while` loop will cost less gas if the condition does not need to be checked in the first iteration:

```solidity
for (uint256 i; i < len; ++i) { ... } // when 'len' is known to be > 0
```
  
could be written as:

```solidity
do { ...; ++i } while (i < len);
```


*Instances: 29*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 231 |  for (uint256 i = 0; i < items.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 341 |  for (uint256 i = 0; i < orders.length; ++i) {
```
GitHub: [231](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L231), [341](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L341)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 197 |  for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 229 |  for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 249 |  for (uint256 i = 0; i < orderHashes.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 260 |  for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
```
GitHub: [197](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L197), [229](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L229), [249](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L249), [260](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L260)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 120 |  for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
```
GitHub: [120](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L120)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Save gas by switching to a do-while loop
  90 |  for (uint256 i = 0; i < itemCount; ++i) {
```
GitHub: [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L90)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 170 |  for (uint256 i = 0; i < order.items.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 176 |  for (uint256 i = 0; i < order.hooks.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 225 |  for (uint256 i = 0; i < metadata.hooks.length; ++i) {
```
GitHub: [170](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L170), [176](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L176), [225](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L225)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 209 |  for (uint256 i; i < offers.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 261 |  for (uint256 i; i < offers.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 337 |  for (uint256 i; i < considerations.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 375 |  for (uint256 i; i < considerations.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 475 |  for (uint256 i = 0; i < hooks.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 567 |  for (uint256 i; i < items.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 599 |  for (uint256 i = 0; i < items.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 695 |  for (uint256 i = 0; i < executions.length; ++i) {
```
GitHub: [209](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L209), [261](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L261), [337](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L337), [375](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L375), [475](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L475), [567](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L567), [599](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L599), [695](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L695)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 205 |  for (uint256 i = 0; i < hooks.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 276 |  for (uint256 i; i < order.items.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 324 |  for (uint256 i = 0; i < orders.length; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 333 |  for (uint256 j = 0; j < orders[i].items.length; ++j) {
```
GitHub: [205](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205), [276](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L276), [324](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L324), [333](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L333)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Save gas by switching to a do-while loop
 438 |  for (uint256 i; i < depLength; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 512 |  for (uint256 i; i < keycodeLen; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 521 |  for (uint256 j; j < policiesLen; ++j) {

     |  // @audit-issue Save gas by switching to a do-while loop
 546 |  for (uint256 i; i < depLength; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 566 |  for (uint256 i = 0; i < reqLength; ++i) {

     |  // @audit-issue Save gas by switching to a do-while loop
 592 |  for (uint256 i; i < depcLength; ++i) {
```
GitHub: [438](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L438), [512](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L512), [521](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L521), [546](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L546), [566](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L566), [592](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L592)


## [G-02] The result of function calls should be cached rather than re-calling the function

The instances below point to the second+ call of the function within a single function.

*Instances: 50*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Called 2 times in _settlePayment()
 255 |  if (orderType.isPayOrder() && !isRentalOver) {

     |  // @audit-issue Called 2 times in _settlePayment()
 268 |  (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
```
GitHub: [255](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L255), [268](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L268)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Called 2 times in configureDependencies()
  48 |  dependencies[0] = toKeycode("STORE");

     |  // @audit-issue Called 2 times in configureDependencies()
  49 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Called 2 times in configureDependencies()
  51 |  dependencies[1] = toKeycode("ESCRW");

     |  // @audit-issue Called 2 times in configureDependencies()
  52 |  ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));

     |  // @audit-issue Called 4 times in requestPermissions()
  73 |  toKeycode("STORE"),

     |  // @audit-issue Called 4 times in requestPermissions()
  77 |  toKeycode("STORE"),

     |  // @audit-issue Called 4 times in requestPermissions()
  80 |  requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);

     |  // @audit-issue Called 4 times in requestPermissions()
  81 |  requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);

     |  // @audit-issue Called 4 times in requestPermissions()
  83 |  requests[4] = Permissions(toKeycode("ESCRW"), ESCRW.skim.selector);

     |  // @audit-issue Called 4 times in requestPermissions()
  84 |  requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);

     |  // @audit-issue Called 4 times in requestPermissions()
  85 |  requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);

     |  // @audit-issue Called 4 times in requestPermissions()
  86 |  requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
```
GitHub: [48](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L48), [49](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L49), [51](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L51), [52](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L52), [73](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L73), [77](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L77), [80](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L80), [81](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L81), [83](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L83), [84](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L84), [85](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L85), [86](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L86)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Called 2 times in configureDependencies()
  80 |  dependencies[0] = toKeycode("STORE");

     |  // @audit-issue Called 2 times in configureDependencies()
  81 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Called 2 times in configureDependencies()
  83 |  dependencies[1] = toKeycode("ESCRW");

     |  // @audit-issue Called 2 times in configureDependencies()
  84 |  ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
```
GitHub: [80](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L80), [81](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L81), [83](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L83), [84](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L84)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Called 2 times in configureDependencies()
  81 |  dependencies[0] = toKeycode("STORE");

     |  // @audit-issue Called 2 times in configureDependencies()
  82 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));
```
GitHub: [81](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L81), [82](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L82)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Called 2 times in configureDependencies()
  71 |  dependencies[0] = toKeycode("STORE");

     |  // @audit-issue Called 2 times in configureDependencies()
  72 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Called 2 times in requestPermissions()
  92 |  requests[0] = Permissions(toKeycode("STORE"), STORE.updateHookPath.selector);

     |  // @audit-issue Called 2 times in requestPermissions()
  93 |  requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);

     |  // @audit-issue Called 5 times in _checkTransaction()
 210 |  _revertSelectorOnActiveRental(selector, from, to, tokenId);

     |  // @audit-issue Called 5 times in _checkTransaction()
 218 |  _revertSelectorOnActiveRental(selector, from, to, tokenId);

     |  // @audit-issue Called 5 times in _checkTransaction()
 226 |  _revertSelectorOnActiveRental(selector, from, to, tokenId);

     |  // @audit-issue Called 5 times in _checkTransaction()
 234 |  _revertSelectorOnActiveRental(selector, from, to, tokenId);

     |  // @audit-issue Called 5 times in _checkTransaction()
 242 |  _revertSelectorOnActiveRental(selector, from, to, tokenId);

     |  // @audit-issue Called 2 times in _checkTransaction()
 254 |  _revertNonWhitelistedExtension(extension);

     |  // @audit-issue Called 2 times in _checkTransaction()
 266 |  _revertNonWhitelistedExtension(extension);
```
GitHub: [71](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L71), [72](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L72), [92](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L92), [93](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L93), [210](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L210), [218](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L218), [226](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L226), [234](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L234), [242](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L242), [254](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L254), [266](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L266)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Called 2 times in configureDependencies()
  71 |  dependencies[0] = toKeycode("STORE");

     |  // @audit-issue Called 2 times in configureDependencies()
  72 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Called 2 times in configureDependencies()
  74 |  dependencies[1] = toKeycode("ESCRW");

     |  // @audit-issue Called 2 times in configureDependencies()
  75 |  ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));

     |  // @audit-issue Called 2 times in requestPermissions()
  95 |  requests[0] = Permissions(toKeycode("STORE"), STORE.removeRentals.selector);

     |  // @audit-issue Called 2 times in requestPermissions()
  96 |  requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);

     |  // @audit-issue Called 2 times in requestPermissions()
  97 |  requests[2] = Permissions(toKeycode("ESCRW"), ESCRW.settlePayment.selector);

     |  // @audit-issue Called 2 times in requestPermissions()
  98 |  requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);

     |  // @audit-issue Called 2 times in _validateRentalCanBeStoped()
 141 |  revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);

     |  // @audit-issue Called 2 times in _validateRentalCanBeStoped()
 149 |  revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender);
```
GitHub: [71](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L71), [72](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L72), [74](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L74), [75](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L75), [95](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L95), [96](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L96), [97](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L97), [98](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L98), [141](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L141), [149](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L149)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Called 5 times in executeAction()
 279 |  ensureContract(target_);

     |  // @audit-issue Called 2 times in executeAction()
 280 |  ensureValidKeycode(Module(target_).KEYCODE());

     |  // @audit-issue Called 2 times in executeAction()
 280 |  ensureValidKeycode(Module(target_).KEYCODE());

     |  // @audit-issue Called 5 times in executeAction()
 283 |  ensureContract(target_);

     |  // @audit-issue Called 2 times in executeAction()
 284 |  ensureValidKeycode(Module(target_).KEYCODE());

     |  // @audit-issue Called 2 times in executeAction()
 284 |  ensureValidKeycode(Module(target_).KEYCODE());

     |  // @audit-issue Called 5 times in executeAction()
 287 |  ensureContract(target_);

     |  // @audit-issue Called 5 times in executeAction()
 290 |  ensureContract(target_);

     |  // @audit-issue Called 5 times in executeAction()
 293 |  ensureContract(target_);
```
GitHub: [279](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L279), [280](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L280), [280](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L280), [283](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L283), [284](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L284), [284](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L284), [287](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L287), [290](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L290), [293](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L293)


## [G-03] `abi.encode()` may be less efficient than `abi.encodePacked()`

See for more information: https://github.com/ConnorBlockchain/Solidity-Encode-Gas-Comparison

*Instances: 9*

```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Consider abi.encodePacked
 129 |  abi.encode(
 130 |      _ITEM_TYPEHASH,
 131 |      item.itemType,
 132 |      item.settleTo,
 133 |      item.token,
 134 |      item.amount,
 135 |      item.identifier
 136 |  )

     |  // @audit-issue Consider abi.encodePacked
 151 |  abi.encode(_HOOK_TYPEHASH, hook.target, hook.itemIndex, hook.extraData)

     |  // @audit-issue Consider abi.encodePacked
 183 |  abi.encode(
 184 |      _RENTAL_ORDER_TYPEHASH,
 185 |      order.seaportOrderHash,
 186 |      keccak256(abi.encodePacked(itemHashes)),
 187 |      keccak256(abi.encodePacked(hookHashes)),
 188 |      order.orderType,
 189 |      order.lender,
 190 |      order.renter,
 191 |      order.startTimestamp,
 192 |      order.endTimestamp
 193 |  )

     |  // @audit-issue Consider abi.encodePacked
 208 |  return keccak256(abi.encode(_ORDER_FULFILLMENT_TYPEHASH, fulfillment.recipient));

     |  // @audit-issue Consider abi.encodePacked
 233 |  abi.encode(
 234 |      _ORDER_METADATA_TYPEHASH,
 235 |      metadata.rentDuration,
 236 |      keccak256(abi.encodePacked(hookHashes))
 237 |  )

     |  // @audit-issue Consider abi.encodePacked
 254 |  abi.encode(
 255 |      _RENT_PAYLOAD_TYPEHASH,
 256 |      _deriveOrderFulfillmentHash(payload.fulfillment),
 257 |      _deriveOrderMetadataHash(payload.metadata),
 258 |      payload.expiration,
 259 |      payload.intendedFulfiller
 260 |  )

     |  // @audit-issue Consider abi.encodePacked
 280 |  abi.encode(
 281 |      _eip712DomainTypeHash,
 282 |      _nameHash,
 283 |      _versionHash,
 284 |      block.chainid,
 285 |      address(this)
 286 |  )

     |  // @audit-issue Consider abi.encodePacked
 374 |  abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)
```
GitHub: [129-136](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L129-L136), [151](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L151), [183-193](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L183-L193), [208](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L208), [233-237](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L233-L237), [254-260](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L254-L260), [280-286](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L280-L286), [374](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L374)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Consider abi.encodePacked
 184 |  uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))
```
GitHub: [184](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L184)


## [G-04] Use `calldata` instead of `memory` for function arguments that are read only

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * `<mem_array>.length`). Using calldata directly, removes the need for such a loop in the contract code and runtime execution.  
                    
If the array is passed to an `internal` function which passes the array to another `internal` function where the array is modified and therefore `memory` is used in the `external` call, it's still more gas-efficient to use `calldata` when the external function uses modifiers, since the modifiers may prevent the `internal` functions from being called. `Structs` have the same overhead as an array of length one.

*Instances: 13*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Function with `memory` params
 215 |  function _settlePayment(
 216 |      Item[] calldata items,
 217 |      OrderType orderType,
 218 |      address lender,
 219 |      address renter,
 220 |      uint256 start,
 221 |      uint256 end
 222 |  ) internal {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 216 |      Item[] calldata items,

     |  // @audit-info Function with `memory` params
     |  // @audit-issue Consinder changing `memory` to `calldata`
 320 |  function settlePayment(RentalOrder calldata order) external onlyByProxy permissioned {

     |  // @audit-info Function with `memory` params
 337 |  function settlePaymentBatch(
 338 |      RentalOrder[] calldata orders
 339 |  ) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 338 |      RentalOrder[] calldata orders
```
GitHub: [216](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L216), [320](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L320), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L338)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info Function with `memory` params
 216 |  function removeRentals(
 217 |      bytes32 orderHash,
 218 |      RentalAssetUpdate[] calldata rentalAssetUpdates
 219 |  ) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 218 |      RentalAssetUpdate[] calldata rentalAssetUpdates

     |  // @audit-info Function with `memory` params
 244 |  function removeRentalsBatch(
 245 |      bytes32[] calldata orderHashes,
 246 |      RentalAssetUpdate[] calldata rentalAssetUpdates
 247 |  ) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 245 |      bytes32[] calldata orderHashes,
     |      // @audit-issue Consinder changing `memory` to `calldata`
 246 |      RentalAssetUpdate[] calldata rentalAssetUpdates
```
GitHub: [218](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L218), [245](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L245), [246](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L246)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-info Function with `memory` params
     |  // @audit-issue Consinder changing `memory` to `calldata`
  71 |  function reclaimRentalOrder(RentalOrder calldata rentalOrder) external {
```
GitHub: [71](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L71)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-info Function with `memory` params
 733 |  function validateOrder(
 734 |      ZoneParameters calldata zoneParams
 735 |  ) external override onlyRole("SEAPORT") returns (bytes4 validOrderMagicValue) {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 734 |      ZoneParameters calldata zoneParams
```
GitHub: [734](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L734)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-info Function with `memory` params
 138 |  function deployRentalSafe(
 139 |      address[] calldata owners,
 140 |      uint256 threshold
 141 |  ) external returns (address safe) {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 139 |      address[] calldata owners,
```
GitHub: [139](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L139)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-info Function with `memory` params
 194 |  function _removeHooks(
 195 |      Hook[] calldata hooks,
 196 |      Item[] calldata rentalItems,
 197 |      address rentalWallet
 198 |  ) internal {
  :  |
     |      // @audit-issue Consinder changing `memory` to `calldata`
 195 |      Hook[] calldata hooks,
     |      // @audit-issue Consinder changing `memory` to `calldata`
 196 |      Item[] calldata rentalItems,

     |  // @audit-info Function with `memory` params
     |  // @audit-issue Consinder changing `memory` to `calldata`
 265 |  function stopRent(RentalOrder calldata order) external {

     |  // @audit-info Function with `memory` params
     |  // @audit-issue Consinder changing `memory` to `calldata`
 313 |  function stopRentBatch(RentalOrder[] calldata orders) external {
```
GitHub: [195](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L195), [196](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L196), [265](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265), [313](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L313)


## [G-05] Inline `internal` functions that are only called once

Saves 20-40 gas per instance. See https://blog.soliditylang.org/2021/03/02/saving-gas-with-simple-inliner/ for more details.

*Instances: 30*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Inline this function, it's only used once
  88 |  function _calculateFee(uint256 amount) internal view returns (uint256) {

     |  // @audit-issue Inline this function, it's only used once
 132 |  function _calculatePaymentProRata(
 133 |      uint256 amount,
 134 |      uint256 elapsedTime,
 135 |      uint256 totalTime
 136 |  ) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {

     |  // @audit-issue Inline this function, it's only used once
 159 |  function _settlePaymentProRata(
 160 |      address token,
 161 |      uint256 amount,
 162 |      address lender,
 163 |      address renter,
 164 |      uint256 elapsedTime,
 165 |      uint256 totalTime
 166 |  ) internal {

     |  // @audit-issue Inline this function, it's only used once
 190 |  function _settlePaymentInFull(
 191 |      address token,
 192 |      uint256 amount,
 193 |      SettleTo settleTo,
 194 |      address lender,
 195 |      address renter
 196 |  ) internal {

     |  // @audit-issue Inline this function, it's only used once
 292 |  function _decreaseDeposit(address token, uint256 amount) internal {

     |  // @audit-issue Inline this function, it's only used once
 304 |  function _increaseDeposit(address token, uint256 amount) internal {
```
GitHub: [88](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88), [132-136](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L132-L136), [159-166](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L159-L166), [190-196](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L190-L196), [292](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L292), [304](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L304)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Inline this function, it's only used once
 125 |  function _deriveItemHash(Item memory item) internal view returns (bytes32) {

     |  // @audit-issue Inline this function, it's only used once
 204 |  function _deriveOrderFulfillmentHash(
 205 |      OrderFulfillment memory fulfillment
 206 |  ) internal view returns (bytes32) {

     |  // @audit-issue Inline this function, it's only used once
 218 |  function _deriveOrderMetadataHash(
 219 |      OrderMetadata memory metadata
 220 |  ) internal view returns (bytes32) {

     |  // @audit-issue Inline this function, it's only used once
 273 |  function _deriveDomainSeparator(
 274 |      bytes32 _eip712DomainTypeHash,
 275 |      bytes32 _nameHash,
 276 |      bytes32 _versionHash
 277 |  ) internal view virtual returns (bytes32) {

     |  // @audit-issue Inline this function, it's only used once
 298 |  function _deriveTypehashes()
 299 |      internal
 300 |      view
 301 |      returns (
 302 |          bytes32 nameHash,
 303 |          bytes32 versionHash,
 304 |          bytes32 eip712DomainTypehash,
 305 |          bytes32 domainSeparator
 306 |      )
 307 |  {

     |  // @audit-issue Inline this function, it's only used once
 339 |  function _deriveRentalTypehashes()
 340 |      internal
 341 |      pure
 342 |      returns (
 343 |          bytes32 itemTypeHash,
 344 |          bytes32 hookTypeHash,
 345 |          bytes32 rentalOrderTypeHash,
 346 |          bytes32 orderFulfillmentTypeHash,
 347 |          bytes32 orderMetadataTypeHash,
 348 |          bytes32 rentPayloadTypeHash
 349 |      )
 350 |  {
```
GitHub: [125](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L125), [204-206](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L204-L206), [218-220](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L218-L220), [273-277](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L273-L277), [298-307](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L298-L307), [339-350](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L339-L350)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Inline this function, it's only used once
 165 |  function _emitRentalOrderStarted(
 166 |      RentalOrder memory order,
 167 |      bytes32 orderHash,
 168 |      bytes memory extraData
 169 |  ) internal {

     |  // @audit-issue Inline this function, it's only used once
 195 |  function _processBaseOrderOffer(
 196 |      Item[] memory rentalItems,
 197 |      SpentItem[] memory offers,
 198 |      uint256 startIndex
 199 |  ) internal pure {

     |  // @audit-issue Inline this function, it's only used once
 247 |  function _processPayOrderOffer(
 248 |      Item[] memory rentalItems,
 249 |      SpentItem[] memory offers,
 250 |      uint256 startIndex
 251 |  ) internal pure {

     |  // @audit-issue Inline this function, it's only used once
 326 |  function _processBaseOrderConsideration(
 327 |      Item[] memory rentalItems,
 328 |      ReceivedItem[] memory considerations,
 329 |      uint256 startIndex
 330 |  ) internal pure {

     |  // @audit-issue Inline this function, it's only used once
 367 |  function _processPayeeOrderConsideration(
 368 |      ReceivedItem[] memory considerations
 369 |  ) internal pure {

     |  // @audit-issue Inline this function, it's only used once
 411 |  function _convertToItems(
 412 |      SpentItem[] memory offers,
 413 |      ReceivedItem[] memory considerations,
 414 |      OrderType orderType
 415 |  ) internal pure returns (Item[] memory items) {

     |  // @audit-issue Inline this function, it's only used once
 464 |  function _addHooks(
 465 |      Hook[] memory hooks,
 466 |      SpentItem[] memory offerItems,
 467 |      address rentalWallet
 468 |  ) internal {

     |  // @audit-issue Inline this function, it's only used once
 530 |  function _rentFromZone(
 531 |      RentPayload memory payload,
 532 |      SeaportPayload memory seaportPayload
 533 |  ) internal {

     |  // @audit-issue Inline this function, it's only used once
 626 |  function _isValidOrderMetadata(
 627 |      OrderMetadata memory metadata,
 628 |      bytes32 zoneHash
 629 |  ) internal view {

     |  // @audit-issue Inline this function, it's only used once
 647 |  function _isValidSafeOwner(address owner, address safe) internal view {

     |  // @audit-issue Inline this function, it's only used once
 691 |  function _executionInvariantChecks(
 692 |      ReceivedItem[] memory executions,
 693 |      address expectedRentalSafe
 694 |  ) internal view {
```
GitHub: [165-169](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L165-L169), [195-199](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L195-L199), [247-251](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L247-L251), [326-330](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L326-L330), [367-369](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L367-L369), [411-415](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L411-L415), [464-468](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L464-L468), [530-533](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L530-L533), [626-629](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L626-L629), [647](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L647), [691-694](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L691-L694)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Inline this function, it's only used once
 356 |  function _installModule(Module newModule_) internal {

     |  // @audit-issue Inline this function, it's only used once
 383 |  function _upgradeModule(Module newModule_) internal {

     |  // @audit-issue Inline this function, it's only used once
 418 |  function _activatePolicy(Policy policy_) internal {

     |  // @audit-issue Inline this function, it's only used once
 457 |  function _deactivatePolicy(Policy policy_) internal {

     |  // @audit-issue Inline this function, it's only used once
 508 |  function _migrateKernel(Kernel newKernel_) internal {

     |  // @audit-issue Inline this function, it's only used once
 540 |  function _reconfigurePolicies(Keycode keycode_) internal {

     |  // @audit-issue Inline this function, it's only used once
 586 |  function _pruneFromDependents(Policy policy_) internal {
```
GitHub: [356](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L356), [383](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L383), [418](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L418), [457](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L457), [508](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L508), [540](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L540), [586](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L586)


## [G-06] Use named return values

Using named return values instead of explicitly calling `return` saves ~13 execution gas per call and >1000 deployment gas per instance.

*Instances: 23*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Provide names for all return parameters
  75 |  function KEYCODE() public pure override returns (Keycode) {

     |  // @audit-issue Provide names for all return parameters
  88 |  function _calculateFee(uint256 amount) internal view returns (uint256) {
```
GitHub: [75](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L75), [88](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Provide names for all return parameters
 103 |  function KEYCODE() public pure override returns (Keycode) {

     |  // @audit-issue Provide names for all return parameters
 118 |  function isRentedOut(
 119 |      address recipient,
 120 |      address token,
 121 |      uint256 identifier
 122 |  ) external view returns (bool) {

     |  // @audit-issue Provide names for all return parameters
 135 |  function contractToHook(address to) external view returns (address) {

     |  // @audit-issue Provide names for all return parameters
 149 |  function hookOnTransaction(address hook) external view returns (bool) {

     |  // @audit-issue Provide names for all return parameters
 159 |  function hookOnStart(address hook) external view returns (bool) {

     |  // @audit-issue Provide names for all return parameters
 169 |  function hookOnStop(address hook) external view returns (bool) {
```
GitHub: [103](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L103), [118-122](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L118-L122), [135](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L135), [149](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L149), [159](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L159), [169](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L169)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Provide names for all return parameters
 107 |  function _recoverSignerFromPayload(
 108 |      bytes32 payloadHash,
 109 |      bytes memory signature
 110 |  ) internal view returns (address) {

     |  // @audit-issue Provide names for all return parameters
 125 |  function _deriveItemHash(Item memory item) internal view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 147 |  function _deriveHookHash(Hook memory hook) internal view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 162 |  function _deriveRentalOrderHash(
 163 |      RentalOrder memory order
 164 |  ) internal view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 204 |  function _deriveOrderFulfillmentHash(
 205 |      OrderFulfillment memory fulfillment
 206 |  ) internal view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 218 |  function _deriveOrderMetadataHash(
 219 |      OrderMetadata memory metadata
 220 |  ) internal view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 248 |  function _deriveRentPayloadHash(
 249 |      RentPayload memory payload
 250 |  ) internal view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 273 |  function _deriveDomainSeparator(
 274 |      bytes32 _eip712DomainTypeHash,
 275 |      bytes32 _nameHash,
 276 |      bytes32 _versionHash
 277 |  ) internal view virtual returns (bytes32) {
```
GitHub: [107-110](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L107-L110), [125](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L125), [147](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L147), [162-164](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L162-L164), [204-206](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L204-L206), [218-220](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L218-L220), [248-250](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L248-L250), [273-277](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L273-L277)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Provide names for all return parameters
 117 |  function domainSeparator() external view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 126 |  function getRentalOrderHash(
 127 |      RentalOrder memory order
 128 |  ) external view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 137 |  function getRentPayloadHash(
 138 |      RentPayload memory payload
 139 |  ) external view returns (bytes32) {

     |  // @audit-issue Provide names for all return parameters
 148 |  function getOrderMetadataHash(
 149 |      OrderMetadata memory metadata
 150 |  ) external view returns (bytes32) {
```
GitHub: [117](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L117), [126-128](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L126-L128), [137-139](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L137-L139), [148-150](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L148-L150)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Provide names for all return parameters
  84 |  function getCreate2Address(
  85 |      bytes32 salt,
  86 |      bytes memory initCode
  87 |  ) public view returns (address) {
```
GitHub: [84-87](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L84-L87)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Provide names for all return parameters
  90 |  function KEYCODE() public pure virtual returns (Keycode);

     |  // @audit-issue Provide names for all return parameters
 180 |  function getModuleAddress(Keycode keycode_) internal view returns (address) {
```
GitHub: [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L90), [180](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L180)


## [G-07] Use a constant for literal `keccak256()` hashes

Since `keccak256()` is deterministic, it is a waste of gas to evaluate `keccak256(<some literal value>)` on every function call. For example, `keccak256("")` costs 30 gas.

*Instances: 1*

```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Store `keccak256(
            abi.encodePacked(
                "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
            )
        )` in a constant
 315 |  eip712DomainTypehash = keccak256(
 316 |      abi.encodePacked(
 317 |          "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
 318 |      )
 319 |  );
```
GitHub: [315-319](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L315-L319)


## [G-08] Refactor modifiers to call a local function

Modifiers code is copied in all instances where it's used, increasing bytecode size. If deployment gas costs are a concern for this contract, refactoring modifiers into functions can reduce bytecode size significantly at the cost of one JUMP.

*Instances: 5*

```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Refactor to a function to save gas
  40 |  modifier onlyKernel() {

     |  // @audit-issue Refactor to a function to save gas
  77 |  modifier permissioned() {

     |  // @audit-issue Refactor to a function to save gas
 130 |  modifier onlyRole(bytes32 role_) {

     |  // @audit-issue Refactor to a function to save gas
 254 |  modifier onlyExecutor() {

     |  // @audit-issue Refactor to a function to save gas
 262 |  modifier onlyAdmin() {
```
GitHub: [40](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L40), [77](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L77), [130](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L130), [254](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L254), [262](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L262)


## [G-09] Inline `modifier`s that are only used once, to save gas

Inline `modifier`s that are only used once, to save gas.

*Instances: 6*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Contains redundant modifiers
  37 |  contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
  :  |
     |      // @audit-issue This is the only invocation of this modifier
  60 |      ) external onlyByProxy onlyUninitialized {
```
GitHub: [60](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L60)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info Contains redundant modifiers
  66 |  contract Storage is Proxiable, Module, StorageBase {
  :  |
     |      // @audit-issue This is the only invocation of this modifier
  88 |      ) external onlyByProxy onlyUninitialized {
```
GitHub: [88](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L88)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-info Contains redundant modifiers
  41 |  contract Create is Policy, Signer, Zone, Accumulator {
  :  |
     |      // @audit-issue This is the only invocation of this modifier
 735 |      ) external override onlyRole("SEAPORT") returns (bytes4 validOrderMagicValue) {
```
GitHub: [735](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L735)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info Contains redundant modifiers
  23 |  abstract contract KernelAdapter {
  :  |
     |      // @audit-issue This is the only invocation of this modifier
  54 |      function changeKernel(Kernel newKernel_) external onlyKernel {

     |  // @audit-info Contains redundant modifiers
  64 |  abstract contract Module is KernelAdapter {
  :  |
     |      // @audit-issue This is the only invocation of this modifier
 106 |      function INIT() external virtual onlyKernel {}

     |  // @audit-info Contains redundant modifiers
 206 |  contract Kernel {
  :  |
     |      // @audit-issue This is the only invocation of this modifier
 277 |      function executeAction(Actions action_, address target_) external onlyExecutor {
```
GitHub: [54](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L54), [106](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L106), [277](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L277)


## [G-10] `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

Using the addition operator instead of plus-equals saves **[113 gas](https://gist.github.com/IllIllI000/cbbfb267425b898e5be734d4008d4fe8)**

*Instances: 5*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Switch to <x> + <y> and <x> - <y>
 294 |  balanceOf[token] -= amount;

     |  // @audit-issue Switch to <x> + <y> and <x> - <y>
 306 |  balanceOf[token] += amount;
```
GitHub: [294](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L294), [306](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L306)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Switch to <x> + <y> and <x> - <y>
 201 |  rentedAssets[asset.rentalId] += asset.amount;

     |  // @audit-issue Switch to <x> + <y> and <x> - <y>
 233 |  rentedAssets[asset.rentalId] -= asset.amount;

     |  // @audit-issue Switch to <x> + <y> and <x> - <y>
 264 |  rentedAssets[asset.rentalId] -= asset.amount;
```
GitHub: [201](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L201), [233](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L233), [264](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L264)


## [G-11] Use Solady library where possible to save gas

Utilizing gas-optimized math functions from libraries like [Solady](https://github.com/Vectorized/solady/blob/main/src/utils/FixedPointMathLib.sol) can lead to more efficient smart contracts.
This is particularly beneficial in contracts where these operations are frequently used.

For example, `(x * WAD) / y` can be replaced with Solady's `divWad()`.


*Instances: 3*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Consider using the Solady library for this arithmetic
  90 |  return (amount * fee) / 10000;

     |  // @audit-issue Consider using the Solady library for this arithmetic
 138 |  uint256 numerator = (amount * elapsedTime) * 1000;

     |  // @audit-issue Consider using the Solady library for this arithmetic
 142 |  renterAmount = ((numerator / totalTime) + 500) / 1000;
```
GitHub: [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L90), [138](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L138), [142](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L142)


## [G-12] Cache multiple accesses of mapping/array values

Caching a mapping's value in a local `storage` or `calldata` variable when the value is accessed multiple times, saves ~42 gas per access due to not having to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and that calculation's associated stack operations. Caching an array's struct avoids recalculating the array offsets into memory/calldata.

*Instances: 72*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Contains multiple accesses to state
 337 |  function settlePaymentBatch(
 338 |      RentalOrder[] calldata orders
 339 |  ) external onlyByProxy permissioned {
  :  |
     |              // @audit-issue Cache this value
 344 |              orders[i].items,
     |              // @audit-issue Cache this value
 345 |              orders[i].orderType,
     |              // @audit-issue Cache this value
 346 |              orders[i].lender,
     |              // @audit-issue Cache this value
 347 |              orders[i].renter,
     |              // @audit-issue Cache this value
 348 |              orders[i].startTimestamp,
     |              // @audit-issue Cache this value
 349 |              orders[i].endTimestamp
```
GitHub: [344](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L344), [345](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L345), [346](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L346), [347](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L347), [348](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L348), [349](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L349)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info Contains multiple accesses to state
 216 |  function removeRentals(
 217 |      bytes32 orderHash,
 218 |      RentalAssetUpdate[] calldata rentalAssetUpdates
 219 |  ) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Cache this value
 221 |      if (!orders[orderHash]) {
  :  |
     |          // @audit-issue Cache this value
 225 |          delete orders[orderHash];

     |  // @audit-info Contains multiple accesses to state
 244 |  function removeRentalsBatch(
 245 |      bytes32[] calldata orderHashes,
 246 |      RentalAssetUpdate[] calldata rentalAssetUpdates
 247 |  ) external onlyByProxy permissioned {
  :  |
     |          // @audit-issue Cache this value
 251 |          if (!orders[orderHashes[i]]) {
  :  |
     |              // @audit-issue Cache this value
 255 |              delete orders[orderHashes[i]];
  :  |
     |          // @audit-issue Cache this value
 251 |          if (!orders[orderHashes[i]]) {
     |              // @audit-issue Cache this value
 252 |              revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
  :  |
     |              // @audit-issue Cache this value
 255 |              delete orders[orderHashes[i]];
```
GitHub: [221](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L221), [225](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L225), [251](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L251), [255](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L255), [251](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L251), [252](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L252), [255](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L255)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-info Contains multiple accesses to state
 464 |  function _addHooks(
 465 |      Hook[] memory hooks,
 466 |      SpentItem[] memory offerItems,
 467 |      address rentalWallet
 468 |  ) internal {
  :  |
     |          // @audit-issue Cache this value
 477 |          target = hooks[i].target;
  :  |
     |          // @audit-issue Cache this value
 485 |          itemIndex = hooks[i].itemIndex;
  :  |
     |                  // @audit-issue Cache this value
 502 |                  hooks[i].extraData

     |  // @audit-info Contains multiple accesses to state
 530 |  function _rentFromZone(
 531 |      RentPayload memory payload,
 532 |      SeaportPayload memory seaportPayload
 533 |  ) internal {
  :  |
     |              // @audit-issue Cache this value
 568 |              if (items[i].isRental()) {
  :  |
     |                      // @audit-issue Cache this value
 572 |                      items[i].toRentalId(payload.fulfillment.recipient),
     |                      // @audit-issue Cache this value
 573 |                      items[i].amount
  :  |
     |              // @audit-issue Cache this value
 600 |              if (items[i].isERC20()) {
     |                  // @audit-issue Cache this value
     |                  // @audit-issue Cache this value
 601 |                  ESCRW.increaseDeposit(items[i].token, items[i].amount);
```
GitHub: [477](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L477), [485](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L485), [502](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L502), [568](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L568), [572](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L572), [573](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L573), [600](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L600), [601](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L601), [601](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L601)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-info Contains multiple accesses to state
 194 |  function _removeHooks(
 195 |      Hook[] calldata hooks,
 196 |      Item[] calldata rentalItems,
 197 |      address rentalWallet
 198 |  ) internal {
  :  |
     |          // @audit-issue Cache this value
 207 |          target = hooks[i].target;
  :  |
     |          // @audit-issue Cache this value
 215 |          itemIndex = hooks[i].itemIndex;
  :  |
     |                  // @audit-issue Cache this value
 232 |                  hooks[i].extraData

     |  // @audit-info Contains multiple accesses to state
 265 |  function stopRent(RentalOrder calldata order) external {
  :  |
     |          // @audit-issue Cache this value
 277 |          if (order.items[i].isRental()) {
  :  |
     |                  // @audit-issue Cache this value
 281 |                  order.items[i].toRentalId(order.rentalWallet),
     |                  // @audit-issue Cache this value
 282 |                  order.items[i].amount

     |  // @audit-info Contains multiple accesses to state
 313 |  function stopRentBatch(RentalOrder[] calldata orders) external {
  :  |
     |              // @audit-issue Cache this value
 327 |              orders[i].orderType,
     |              // @audit-issue Cache this value
 328 |              orders[i].endTimestamp,
     |              // @audit-issue Cache this value
 329 |              orders[i].lender
  :  |
     |          // @audit-issue Cache this value
 333 |          for (uint256 j = 0; j < orders[i].items.length; ++j) {
  :  |
     |              // @audit-issue Cache this value
 335 |              if (orders[i].items[j].isRental()) {
  :  |
     |                      // @audit-issue Cache this value
     |                      // @audit-issue Cache this value
 338 |                      orders[i].items[j].toRentalId(orders[i].rentalWallet),
     |                      // @audit-issue Cache this value
 339 |                      orders[i].items[j].amount
  :  |
     |          // @audit-issue Cache this value
 345 |          orderHashes[i] = _deriveRentalOrderHash(orders[i]);
  :  |
     |          // @audit-issue Cache this value
 348 |          if (orders[i].hooks.length > 0) {
     |              // @audit-issue Cache this value
     |              // @audit-issue Cache this value
     |              // @audit-issue Cache this value
 349 |              _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet);
  :  |
     |          // @audit-issue Cache this value
 353 |          _reclaimRentedItems(orders[i]);
  :  |
     |              // @audit-issue Cache this value
 335 |              if (orders[i].items[j].isRental()) {
  :  |
     |                      // @audit-issue Cache this value
 338 |                      orders[i].items[j].toRentalId(orders[i].rentalWallet),
     |                      // @audit-issue Cache this value
 339 |                      orders[i].items[j].amount
  :  |
     |          // @audit-issue Cache this value
 345 |          orderHashes[i] = _deriveRentalOrderHash(orders[i]);
  :  |
     |          // @audit-issue Cache this value
 356 |          _emitRentalOrderStopped(orderHashes[i], msg.sender);
```
GitHub: [207](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L207), [215](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L215), [232](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L232), [277](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L277), [281](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L281), [282](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L282), [327](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L327), [328](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L328), [329](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L329), [333](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L333), [335](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L335), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L338), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L338), [339](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L339), [345](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L345), [348](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L348), [349](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L349), [349](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L349), [349](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L349), [353](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L353), [335](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L335), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L338), [339](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L339), [345](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L345), [356](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L356)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-info Contains multiple accesses to state
  32 |  function deploy(
  33 |      bytes32 salt,
  34 |      bytes memory initCode
  35 |  ) external payable returns (address deploymentAddress) {
  :  |
     |      // @audit-issue Cache this value
  45 |      if (deployed[targetDeploymentAddress]) {
  :  |
     |      // @audit-issue Cache this value
  50 |      deployed[targetDeploymentAddress] = true;
```
GitHub: [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L45), [50](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L50)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info Contains multiple accesses to state
 310 |  function grantRole(Role role_, address addr_) public onlyAdmin {
  :  |
     |      // @audit-issue Cache this value
 312 |      if (hasRole[addr_][role_])
  :  |
     |      // @audit-issue Cache this value
 322 |      hasRole[addr_][role_] = true;
  :  |
     |      // @audit-issue Cache this value
 312 |      if (hasRole[addr_][role_])
  :  |
     |      // @audit-issue Cache this value
 322 |      hasRole[addr_][role_] = true;
  :  |
     |      // @audit-issue Cache this value
     |      // @audit-issue Cache this value
 319 |      if (!isRole[role_]) isRole[role_] = true;

     |  // @audit-info Contains multiple accesses to state
 333 |  function revokeRole(Role role_, address addr_) public onlyAdmin {
  :  |
     |      // @audit-issue Cache this value
 338 |      if (!hasRole[addr_][role_])
  :  |
     |      // @audit-issue Cache this value
 342 |      hasRole[addr_][role_] = false;
  :  |
     |      // @audit-issue Cache this value
 338 |      if (!hasRole[addr_][role_])
  :  |
     |      // @audit-issue Cache this value
 342 |      hasRole[addr_][role_] = false;

     |  // @audit-info Contains multiple accesses to state
 356 |  function _installModule(Module newModule_) internal {
  :  |
     |      // @audit-issue Cache this value
 361 |      if (address(getModuleForKeycode[keycode]) != address(0)) {
  :  |
     |      // @audit-issue Cache this value
 366 |      getModuleForKeycode[keycode] = newModule_;

     |  // @audit-info Contains multiple accesses to state
 383 |  function _upgradeModule(Module newModule_) internal {
  :  |
     |      // @audit-issue Cache this value
 388 |      Module oldModule = getModuleForKeycode[keycode];
  :  |
     |      // @audit-issue Cache this value
 403 |      getModuleForKeycode[keycode] = newModule_;

     |  // @audit-info Contains multiple accesses to state
 418 |  function _activatePolicy(Policy policy_) internal {
  :  |
     |          // @audit-issue Cache this value
 442 |          moduleDependents[keycode].push(policy_);
  :  |
     |          // @audit-issue Cache this value
 445 |          getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1;

     |  // @audit-info Contains multiple accesses to state
 457 |  function _deactivatePolicy(Policy policy_) internal {
  :  |
     |      // @audit-issue Cache this value
 466 |      uint256 idx = getPolicyIndex[policy_];
  :  |
     |      // @audit-issue Cache this value
 482 |      delete getPolicyIndex[policy_];

     |  // @audit-info Contains multiple accesses to state
 586 |  function _pruneFromDependents(Policy policy_) internal {
  :  |
     |          // @audit-issue Cache this value
 598 |          uint256 origIndex = getDependentIndex[keycode][policy_];
  :  |
     |          // @audit-issue Cache this value
 614 |          delete getDependentIndex[keycode][policy_];
  :  |
     |          // @audit-issue Cache this value
 598 |          uint256 origIndex = getDependentIndex[keycode][policy_];
  :  |
     |          // @audit-issue Cache this value
 611 |          getDependentIndex[keycode][lastPolicy] = origIndex;
  :  |
     |          // @audit-issue Cache this value
 614 |          delete getDependentIndex[keycode][policy_];
```
GitHub: [312](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L312), [322](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L322), [312](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L312), [322](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L322), [319](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L319), [319](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L319), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L338), [342](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L342), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L338), [342](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L342), [361](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L361), [366](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L366), [388](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L388), [403](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L403), [442](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L442), [445](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L445), [466](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L466), [482](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L482), [598](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L598), [614](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L614), [598](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L598), [611](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L611), [614](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L614)


## [G-13] Cache state variables accessed multiple times in the same function

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Instances: 33*

```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info This function accesses state variable `orders` multiple times
 216 |  function removeRentals(
  :  |
     |          // @audit-issue `orders` was first accessed on line 221
 225 |          delete orders[orderHash];

     |  // @audit-info This function accesses state variable `orders` multiple times
 244 |  function removeRentalsBatch(
  :  |
     |              // @audit-issue `orders` was first accessed on line 251
 255 |              delete orders[orderHashes[i]];

     |  // @audit-info This function accesses state variable `totalSafes` multiple times
 274 |  function addRentalSafe(address safe) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue `totalSafes` was first accessed on line 276
 282 |      totalSafes = newSafeCount;
```
GitHub: [225](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L225), [255](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L255), [282](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L282)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-info This function accesses state variable `STORE` multiple times
  64 |  function requestPermissions()
  :  |
     |          // @audit-issue `STORE` was first accessed on line 74
  78 |          STORE.toggleWhitelistDelegate.selector
  :  |
     |      // @audit-issue `STORE` was first accessed on line 74
  80 |      requests[2] = Permissions(toKeycode("STORE"), STORE.upgrade.selector);
     |      // @audit-issue `STORE` was first accessed on line 74
  81 |      requests[3] = Permissions(toKeycode("STORE"), STORE.freeze.selector);

     |  // @audit-info This function accesses state variable `ESCRW` multiple times
  64 |  function requestPermissions()
  :  |
     |      // @audit-issue `ESCRW` was first accessed on line 83
  84 |      requests[5] = Permissions(toKeycode("ESCRW"), ESCRW.setFee.selector);
     |      // @audit-issue `ESCRW` was first accessed on line 83
  85 |      requests[6] = Permissions(toKeycode("ESCRW"), ESCRW.upgrade.selector);
     |      // @audit-issue `ESCRW` was first accessed on line 83
  86 |      requests[7] = Permissions(toKeycode("ESCRW"), ESCRW.freeze.selector);
```
GitHub: [78](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L78), [80](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L80), [81](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L81), [84](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L84), [85](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L85), [86](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L86)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-info This function accesses state variable `STORE` multiple times
 138 |  function deployRentalSafe(
  :  |
     |      // @audit-issue `STORE` was first accessed on line 184
 189 |      STORE.addRentalSafe(safe);
```
GitHub: [189](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L189)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-info This function accesses state variable `STORE` multiple times
  84 |  function requestPermissions()
  :  |
     |      // @audit-issue `STORE` was first accessed on line 92
  93 |      requests[1] = Permissions(toKeycode("STORE"), STORE.updateHookStatus.selector);

     |  // @audit-info This function accesses state variable `STORE` multiple times
 309 |  function checkTransaction(
  :  |
     |      // @audit-issue `STORE` was first accessed on line 324
 334 |      address hook = STORE.contractToHook(to);
     |      // @audit-issue `STORE` was first accessed on line 324
 335 |      bool isActive = STORE.hookOnTransaction(hook);
```
GitHub: [93](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L93), [334](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L334), [335](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L335)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-info This function accesses state variable `STORE` multiple times
  87 |  function requestPermissions()
  :  |
     |      // @audit-issue `STORE` was first accessed on line 95
  96 |      requests[1] = Permissions(toKeycode("STORE"), STORE.removeRentalsBatch.selector);

     |  // @audit-info This function accesses state variable `ESCRW` multiple times
  87 |  function requestPermissions()
  :  |
     |      // @audit-issue `ESCRW` was first accessed on line 97
  98 |      requests[3] = Permissions(toKeycode("ESCRW"), ESCRW.settlePaymentBatch.selector);
```
GitHub: [96](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L96), [98](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L98)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-info This function accesses state variable `deployed` multiple times
  32 |  function deploy(
  :  |
     |      // @audit-issue `deployed` was first accessed on line 45
  50 |      deployed[targetDeploymentAddress] = true;
```
GitHub: [50](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L50)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info This function accesses state variable `hasRole` multiple times
 310 |  function grantRole(Role role_, address addr_) public onlyAdmin {
  :  |
     |      // @audit-issue `hasRole` was first accessed on line 312
 322 |      hasRole[addr_][role_] = true;

     |  // @audit-info This function accesses state variable `isRole` multiple times
 310 |  function grantRole(Role role_, address addr_) public onlyAdmin {
  :  |
     |      // @audit-issue `isRole` was first accessed on line 319
 319 |      if (!isRole[role_]) isRole[role_] = true;

     |  // @audit-info This function accesses state variable `hasRole` multiple times
 333 |  function revokeRole(Role role_, address addr_) public onlyAdmin {
  :  |
     |      // @audit-issue `hasRole` was first accessed on line 338
 342 |      hasRole[addr_][role_] = false;

     |  // @audit-info This function accesses state variable `getModuleForKeycode` multiple times
 356 |  function _installModule(Module newModule_) internal {
  :  |
     |      // @audit-issue `getModuleForKeycode` was first accessed on line 361
 366 |      getModuleForKeycode[keycode] = newModule_;

     |  // @audit-info This function accesses state variable `getModuleForKeycode` multiple times
 383 |  function _upgradeModule(Module newModule_) internal {
  :  |
     |      // @audit-issue `getModuleForKeycode` was first accessed on line 388
 403 |      getModuleForKeycode[keycode] = newModule_;

     |  // @audit-info This function accesses state variable `getKeycodeForModule` multiple times
 383 |  function _upgradeModule(Module newModule_) internal {
  :  |
     |      // @audit-issue `getKeycodeForModule` was first accessed on line 397
 400 |      getKeycodeForModule[newModule_] = keycode;

     |  // @audit-info This function accesses state variable `activePolicies` multiple times
 418 |  function _activatePolicy(Policy policy_) internal {
  :  |
     |      // @audit-issue `activePolicies` was first accessed on line 428
 431 |      getPolicyIndex[policy_] = activePolicies.length - 1;

     |  // @audit-info This function accesses state variable `moduleDependents` multiple times
 418 |  function _activatePolicy(Policy policy_) internal {
  :  |
     |          // @audit-issue `moduleDependents` was first accessed on line 442
 445 |          getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1;

     |  // @audit-info This function accesses state variable `getPolicyIndex` multiple times
 457 |  function _deactivatePolicy(Policy policy_) internal {
  :  |
     |      // @audit-issue `getPolicyIndex` was first accessed on line 466
 479 |      getPolicyIndex[lastPolicy] = idx;
  :  |
     |      // @audit-issue `getPolicyIndex` was first accessed on line 466
 482 |      delete getPolicyIndex[policy_];

     |  // @audit-info This function accesses state variable `activePolicies` multiple times
 457 |  function _deactivatePolicy(Policy policy_) internal {
  :  |
     |      // @audit-issue `activePolicies` was first accessed on line 469
 469 |      Policy lastPolicy = activePolicies[activePolicies.length - 1];
  :  |
     |      // @audit-issue `activePolicies` was first accessed on line 469
 472 |      activePolicies[idx] = lastPolicy;
  :  |
     |      // @audit-issue `activePolicies` was first accessed on line 469
 475 |      activePolicies.pop();

     |  // @audit-info This function accesses state variable `allKeycodes` multiple times
 508 |  function _migrateKernel(Kernel newKernel_) internal {
  :  |
     |          // @audit-issue `allKeycodes` was first accessed on line 509
 514 |          Module module = Module(getModuleForKeycode[allKeycodes[i]]);

     |  // @audit-info This function accesses state variable `activePolicies` multiple times
 508 |  function _migrateKernel(Kernel newKernel_) internal {
  :  |
     |          // @audit-issue `activePolicies` was first accessed on line 520
 523 |          Policy policy = activePolicies[j];

     |  // @audit-info This function accesses state variable `getDependentIndex` multiple times
 586 |  function _pruneFromDependents(Policy policy_) internal {
  :  |
     |          // @audit-issue `getDependentIndex` was first accessed on line 598
 611 |          getDependentIndex[keycode][lastPolicy] = origIndex;
  :  |
     |          // @audit-issue `getDependentIndex` was first accessed on line 598
 614 |          delete getDependentIndex[keycode][policy_];
```
GitHub: [322](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L322), [319](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L319), [342](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L342), [366](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L366), [403](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L403), [400](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L400), [431](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L431), [445](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L445), [479](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L479), [482](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L482), [469](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L469), [472](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L472), [475](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L475), [514](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L514), [523](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L523), [611](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L611), [614](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L614)


## [G-14] Avoid updating storage when the value hasn't changed

If the old value is equal to the new value, not re-storing the value will avoid a `Gsreset` (2900 gas), potentially at the expense of a `Gcoldsload` (2100 gas) or a `Gwarmaccess` (100 gas). Min = `Gsreset` - `Gcoldsload`, Max = `Gsreset` - `Gwarmaccess`.

*Instances: 6*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Contains state variable assignments
 380 |  function setFee(uint256 feeNumerator) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Consider checking if the value has changed first
 387 |      fee = feeNumerator;
```
GitHub: [387](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L387)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info Contains state variable assignments
 294 |  function updateHookPath(address to, address hook) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Consider checking if the value has changed first
 302 |      _contractToHook[to] = hook;

     |  // @audit-info Contains state variable assignments
 313 |  function updateHookStatus(
 314 |      address hook,
 315 |      uint8 bitmap
 316 |  ) external onlyByProxy permissioned {
  :  |
     |      // @audit-issue Consider checking if the value has changed first
 325 |      hookStatus[hook] = bitmap;
```
GitHub: [302](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L302), [325](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L325)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info Contains state variable assignments
  54 |  function changeKernel(Kernel newKernel_) external onlyKernel {
     |      // @audit-issue Consider checking if the value has changed first
  55 |      kernel = newKernel_;

     |  // @audit-info Contains state variable assignments
 192 |  function setActiveStatus(bool activate_) external onlyKernel {
     |      // @audit-issue Consider checking if the value has changed first
 193 |      isActive = activate_;

     |  // @audit-info Contains state variable assignments
 560 |  function _setPolicyPermissions(
 561 |      Policy policy_,
 562 |      Permissions[] memory requests_,
 563 |      bool grant_
 564 |  ) internal {
  :  |
     |          // @audit-issue Consider checking if the value has changed first
 569 |          modulePermissions[request.keycode][policy_][request.funcSelector] = grant_;
```
GitHub: [55](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L55), [193](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L193), [569](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L569)


## [G-15] Using `storage` instead of `memory` for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a `memory` variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (**2100 gas**) for *each* field of the struct/array. If the fields are read from the new memory variable, they incur an additional `MLOAD` rather than a cheap stack read. Instead of declearing the variable with the `memory` keyword, declaring the variable with the `storage` keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a `memory` variable, is if the full struct/array is being returned by the function, is being passed to a function that requires `memory`, or if the array/struct is being read from another `memory` array/struct.

*Instances: 71*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue This function accesses state variables multiple times
 233 |  Item memory item = items[i];
```
GitHub: [233](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L233)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue This function accesses state variables multiple times
 191 |  RentalAssetUpdate[] memory rentalAssetUpdates

     |  // @audit-issue This function accesses state variables multiple times
 198 |  RentalAssetUpdate memory asset = rentalAssetUpdates[i];

     |  // @audit-issue This function accesses state variables multiple times
 230 |  RentalAssetUpdate memory asset = rentalAssetUpdates[i];

     |  // @audit-issue This function accesses state variables multiple times
 261 |  RentalAssetUpdate memory asset = rentalAssetUpdates[i];
```
GitHub: [191](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L191), [198](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L198), [230](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L230), [261](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L261)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue This function accesses state variables multiple times
  98 |  ) internal pure returns (RentalAssetUpdate[] memory updates) {
```
GitHub: [98](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L98)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue This function accesses state variables multiple times
  32 |  function _transferERC721(Item memory item, address recipient) private {

     |  // @audit-issue This function accesses state variables multiple times
  42 |  function _transferERC1155(Item memory item, address recipient) private {

     |  // @audit-issue This function accesses state variables multiple times
  91 |  Item memory item = rentalOrder.items[i];
```
GitHub: [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L32), [42](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L42), [91](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L91)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue This function accesses state variables multiple times
 125 |  function _deriveItemHash(Item memory item) internal view returns (bytes32) {

     |  // @audit-issue This function accesses state variables multiple times
 147 |  function _deriveHookHash(Hook memory hook) internal view returns (bytes32) {

     |  // @audit-issue This function accesses state variables multiple times
 163 |  RentalOrder memory order

     |  // @audit-issue This function accesses state variables multiple times
 166 |  bytes32[] memory itemHashes = new bytes32[](order.items.length);

     |  // @audit-issue This function accesses state variables multiple times
 167 |  bytes32[] memory hookHashes = new bytes32[](order.hooks.length);

     |  // @audit-issue This function accesses state variables multiple times
 205 |  OrderFulfillment memory fulfillment

     |  // @audit-issue This function accesses state variables multiple times
 219 |  OrderMetadata memory metadata

     |  // @audit-issue This function accesses state variables multiple times
 222 |  bytes32[] memory hookHashes = new bytes32[](metadata.hooks.length);

     |  // @audit-issue This function accesses state variables multiple times
 249 |  RentPayload memory payload
```
GitHub: [125](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L125), [147](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L147), [163](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L163), [166](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L166), [167](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L167), [205](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L205), [219](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L219), [222](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L222), [249](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L249)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue This function accesses state variables multiple times
  44 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue This function accesses state variables multiple times
  69 |  returns (Permissions[] memory requests)
```
GitHub: [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L44), [69](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L69)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue This function accesses state variables multiple times
  76 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue This function accesses state variables multiple times
 101 |  returns (Permissions[] memory requests)

     |  // @audit-issue This function accesses state variables multiple times
 127 |  RentalOrder memory order

     |  // @audit-issue This function accesses state variables multiple times
 138 |  RentPayload memory payload

     |  // @audit-issue This function accesses state variables multiple times
 149 |  OrderMetadata memory metadata

     |  // @audit-issue This function accesses state variables multiple times
 166 |  RentalOrder memory order,

     |  // @audit-issue This function accesses state variables multiple times
 196 |  Item[] memory rentalItems,

     |  // @audit-issue This function accesses state variables multiple times
 197 |  SpentItem[] memory offers,

     |  // @audit-issue This function accesses state variables multiple times
 211 |  SpentItem memory offer = offers[i];

     |  // @audit-issue This function accesses state variables multiple times
 248 |  Item[] memory rentalItems,

     |  // @audit-issue This function accesses state variables multiple times
 249 |  SpentItem[] memory offers,

     |  // @audit-issue This function accesses state variables multiple times
 263 |  SpentItem memory offer = offers[i];

     |  // @audit-issue This function accesses state variables multiple times
 327 |  Item[] memory rentalItems,

     |  // @audit-issue This function accesses state variables multiple times
 328 |  ReceivedItem[] memory considerations,

     |  // @audit-issue This function accesses state variables multiple times
 339 |  ReceivedItem memory consideration = considerations[i];

     |  // @audit-issue This function accesses state variables multiple times
 368 |  ReceivedItem[] memory considerations

     |  // @audit-issue This function accesses state variables multiple times
 377 |  ReceivedItem memory consideration = considerations[i];

     |  // @audit-issue This function accesses state variables multiple times
 412 |  SpentItem[] memory offers,

     |  // @audit-issue This function accesses state variables multiple times
 413 |  ReceivedItem[] memory considerations,

     |  // @audit-issue This function accesses state variables multiple times
 415 |  ) internal pure returns (Item[] memory items) {

     |  // @audit-issue This function accesses state variables multiple times
 465 |  Hook[] memory hooks,

     |  // @audit-issue This function accesses state variables multiple times
 466 |  SpentItem[] memory offerItems,

     |  // @audit-issue This function accesses state variables multiple times
 472 |  SpentItem memory offer;

     |  // @audit-issue This function accesses state variables multiple times
 531 |  RentPayload memory payload,

     |  // @audit-issue This function accesses state variables multiple times
 532 |  SeaportPayload memory seaportPayload

     |  // @audit-issue This function accesses state variables multiple times
 548 |  Item[] memory items = _convertToItems(

     |  // @audit-issue This function accesses state variables multiple times
 579 |  RentalOrder memory order = RentalOrder({

     |  // @audit-issue This function accesses state variables multiple times
 627 |  OrderMetadata memory metadata,

     |  // @audit-issue This function accesses state variables multiple times
 667 |  ReceivedItem memory execution,

     |  // @audit-issue This function accesses state variables multiple times
 692 |  ReceivedItem[] memory executions,

     |  // @audit-issue This function accesses state variables multiple times
 696 |  ReceivedItem memory execution = executions[i];

     |  // @audit-issue This function accesses state variables multiple times
 737 |  (RentPayload memory payload, bytes memory signature) = abi.decode(

     |  // @audit-issue This function accesses state variables multiple times
 743 |  SeaportPayload memory seaportPayload = SeaportPayload({
```
GitHub: [76](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L76), [101](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L101), [127](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L127), [138](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L138), [149](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L149), [166](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L166), [196](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L196), [197](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L197), [211](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L211), [248](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L248), [249](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L249), [263](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L263), [327](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L327), [328](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L328), [339](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L339), [368](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L368), [377](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L377), [412](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L412), [413](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L413), [415](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L415), [465](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L465), [466](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L466), [472](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L472), [531](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L531), [532](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L532), [548](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L548), [579](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L579), [627](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L627), [667](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L667), [692](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L692), [696](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L696), [737](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L737), [743](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L743)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue This function accesses state variables multiple times
  77 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue This function accesses state variables multiple times
  99 |  returns (Permissions[] memory requests)
```
GitHub: [77](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L77), [99](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L99)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue This function accesses state variables multiple times
  67 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue This function accesses state variables multiple times
  89 |  returns (Permissions[] memory requests)
```
GitHub: [67](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L67), [89](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L89)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue This function accesses state variables multiple times
  67 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue This function accesses state variables multiple times
  92 |  returns (Permissions[] memory requests)

     |  // @audit-issue This function accesses state variables multiple times
 166 |  function _reclaimRentedItems(RentalOrder memory order) internal {

     |  // @audit-issue This function accesses state variables multiple times
 202 |  Item memory item;

     |  // @audit-issue This function accesses state variables multiple times
 315 |  bytes32[] memory orderHashes = new bytes32[](orders.length);
```
GitHub: [67](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L67), [92](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L92), [166](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L166), [202](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L202), [315](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L315)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue This function accesses state variables multiple times
 152 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue This function accesses state variables multiple times
 171 |  returns (Permissions[] memory requests)

     |  // @audit-issue This function accesses state variables multiple times
 424 |  Permissions[] memory requests = policy_.requestPermissions();

     |  // @audit-issue This function accesses state variables multiple times
 434 |  Keycode[] memory dependencies = policy_.configureDependencies();

     |  // @audit-issue This function accesses state variables multiple times
 462 |  Permissions[] memory requests = policy_.requestPermissions();

     |  // @audit-issue This function accesses state variables multiple times
 542 |  Policy[] memory dependents = moduleDependents[keycode_];

     |  // @audit-issue This function accesses state variables multiple times
 562 |  Permissions[] memory requests_,

     |  // @audit-issue This function accesses state variables multiple times
 568 |  Permissions memory request = requests_[i];

     |  // @audit-issue This function accesses state variables multiple times
 588 |  Keycode[] memory dependencies = policy_.configureDependencies();
```
GitHub: [152](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L152), [171](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L171), [424](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L424), [434](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L434), [462](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L462), [542](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L542), [562](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L562), [568](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L568), [588](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L588)


## [G-16] Use assembly to write storage values

Instead of:
                    
```solidity
owner = _newOwner
```

write:

```solidity
assembly { sstore(owner.slot, _newOwner) }
```


*Instances: 56*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  61 |  kernel = kernel_;

     |  // @audit-issue Use assembly `sstore` to save gas
  62 |  initialized = true;

     |  // @audit-issue Use assembly `sstore` to save gas
 294 |  balanceOf[token] -= amount;

     |  // @audit-issue Use assembly `sstore` to save gas
 306 |  balanceOf[token] += amount;

     |  // @audit-issue Use assembly `sstore` to save gas
 387 |  fee = feeNumerator;
```
GitHub: [61](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L61), [62](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L62), [294](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L294), [306](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L306), [387](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L387)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  89 |  kernel = kernel_;

     |  // @audit-issue Use assembly `sstore` to save gas
  90 |  initialized = true;

     |  // @audit-issue Use assembly `sstore` to save gas
 194 |  orders[orderHash] = true;

     |  // @audit-issue Use assembly `sstore` to save gas
 201 |  rentedAssets[asset.rentalId] += asset.amount;

     |  // @audit-issue Use assembly `sstore` to save gas
 233 |  rentedAssets[asset.rentalId] -= asset.amount;

     |  // @audit-issue Use assembly `sstore` to save gas
 264 |  rentedAssets[asset.rentalId] -= asset.amount;

     |  // @audit-issue Use assembly `sstore` to save gas
 279 |  deployedSafes[safe] = newSafeCount;

     |  // @audit-issue Use assembly `sstore` to save gas
 282 |  totalSafes = newSafeCount;

     |  // @audit-issue Use assembly `sstore` to save gas
 302 |  _contractToHook[to] = hook;

     |  // @audit-issue Use assembly `sstore` to save gas
 325 |  hookStatus[hook] = bitmap;

     |  // @audit-issue Use assembly `sstore` to save gas
 338 |  whitelistedDelegates[delegate] = isEnabled;

     |  // @audit-issue Use assembly `sstore` to save gas
 351 |  whitelistedExtensions[extension] = isEnabled;
```
GitHub: [89](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L89), [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L90), [194](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L194), [201](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L201), [233](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L233), [264](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L264), [279](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L279), [282](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L282), [302](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L302), [325](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L325), [338](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L338), [351](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L351)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  23 |  original = address(this);
```
GitHub: [23](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L23)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  46 |  (
  47 |      _NAME_HASH,
  48 |      _VERSION_HASH,
  49 |      _EIP_712_DOMAIN_TYPEHASH,
  50 |      _DOMAIN_SEPARATOR
  51 |  ) = _deriveTypehashes();

     |  // @audit-issue Use assembly `sstore` to save gas
  54 |  (
  55 |      _ITEM_TYPEHASH,
  56 |      _HOOK_TYPEHASH,
  57 |      _RENTAL_ORDER_TYPEHASH,
  58 |      _ORDER_FULFILLMENT_TYPEHASH,
  59 |      _ORDER_METADATA_TYPEHASH,
  60 |      _RENT_PAYLOAD_TYPEHASH
  61 |  ) = _deriveRentalTypehashes();

     |  // @audit-issue Use assembly `sstore` to save gas
  64 |  _CHAIN_ID = block.chainid;
```
GitHub: [46-51](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L46-L51), [54-61](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L54-L61), [64](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L64)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  49 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Use assembly `sstore` to save gas
  52 |  ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
```
GitHub: [49](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L49), [52](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L52)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  81 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Use assembly `sstore` to save gas
  84 |  ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
```
GitHub: [81](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L81), [84](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L84)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  57 |  stopPolicy = stopPolicy_;

     |  // @audit-issue Use assembly `sstore` to save gas
  58 |  guardPolicy = guardPolicy_;

     |  // @audit-issue Use assembly `sstore` to save gas
  59 |  fallbackHandler = fallbackHandler_;

     |  // @audit-issue Use assembly `sstore` to save gas
  60 |  safeProxyFactory = safeProxyFactory_;

     |  // @audit-issue Use assembly `sstore` to save gas
  61 |  safeSingleton = safeSingleton_;

     |  // @audit-issue Use assembly `sstore` to save gas
  82 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));
```
GitHub: [57](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L57), [58](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L58), [59](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L59), [60](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L60), [61](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L61), [82](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L82)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  72 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));
```
GitHub: [72](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L72)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  72 |  STORE = Storage(getModuleAddress(toKeycode("STORE")));

     |  // @audit-issue Use assembly `sstore` to save gas
  75 |  ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
```
GitHub: [72](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L72), [75](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L75)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  50 |  deployed[targetDeploymentAddress] = true;
```
GitHub: [50](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L50)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Use assembly `sstore` to save gas
  34 |  kernel = kernel_;

     |  // @audit-issue Use assembly `sstore` to save gas
  55 |  kernel = newKernel_;

     |  // @audit-issue Use assembly `sstore` to save gas
 193 |  isActive = activate_;

     |  // @audit-issue Use assembly `sstore` to save gas
 243 |  executor = _executor;

     |  // @audit-issue Use assembly `sstore` to save gas
 244 |  admin = _admin;

     |  // @audit-issue Use assembly `sstore` to save gas
 296 |  executor = target_;

     |  // @audit-issue Use assembly `sstore` to save gas
 298 |  admin = target_;

     |  // @audit-issue Use assembly `sstore` to save gas
 319 |  if (!isRole[role_]) isRole[role_] = true;

     |  // @audit-issue Use assembly `sstore` to save gas
 322 |  hasRole[addr_][role_] = true;

     |  // @audit-issue Use assembly `sstore` to save gas
 342 |  hasRole[addr_][role_] = false;

     |  // @audit-issue Use assembly `sstore` to save gas
 366 |  getModuleForKeycode[keycode] = newModule_;

     |  // @audit-issue Use assembly `sstore` to save gas
 369 |  getKeycodeForModule[newModule_] = keycode;

     |  // @audit-issue Use assembly `sstore` to save gas
 397 |  getKeycodeForModule[oldModule] = Keycode.wrap(bytes5(0));

     |  // @audit-issue Use assembly `sstore` to save gas
 400 |  getKeycodeForModule[newModule_] = keycode;

     |  // @audit-issue Use assembly `sstore` to save gas
 403 |  getModuleForKeycode[keycode] = newModule_;

     |  // @audit-issue Use assembly `sstore` to save gas
 431 |  getPolicyIndex[policy_] = activePolicies.length - 1;

     |  // @audit-issue Use assembly `sstore` to save gas
 445 |  getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1;

     |  // @audit-issue Use assembly `sstore` to save gas
 472 |  activePolicies[idx] = lastPolicy;

     |  // @audit-issue Use assembly `sstore` to save gas
 479 |  getPolicyIndex[lastPolicy] = idx;

     |  // @audit-issue Use assembly `sstore` to save gas
 569 |  modulePermissions[request.keycode][policy_][request.funcSelector] = grant_;

     |  // @audit-issue Use assembly `sstore` to save gas
 611 |  getDependentIndex[keycode][lastPolicy] = origIndex;
```
GitHub: [34](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L34), [55](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L55), [193](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L193), [243](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L243), [244](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L244), [296](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L296), [298](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L298), [319](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L319), [322](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L322), [342](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L342), [366](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L366), [369](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L369), [397](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L397), [400](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L400), [403](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L403), [431](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L431), [445](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L445), [472](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L472), [479](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L479), [569](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L569), [611](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L611)


## [G-17] Redundant type conversion

Casting a variable to its own type is redundant and a waste of gas.

*Instances: 1*

```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue `Module(getModuleForKeycode[allKeycodes[i]])` is a redundant type cast
 514 |  Module module = Module(getModuleForKeycode[allKeycodes[i]]);
```
GitHub: [514](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L514)


## [G-18] Not using the named return variable is confusing and can waste gas

Consider changing the variable to be an unnamed one, since the variable is never assigned, nor is it returned by name. If the optimizer is not turned on, leaving the code as it is will also waste gas for the stack variable.

*Instances: 8*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Remove unused variable `major` to save gas
  68 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {

     |  // @audit-issue Remove unused variable `minor` to save gas
  68 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {
```
GitHub: [68](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L68), [68](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L68)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Remove unused variable `major` to save gas
  96 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {

     |  // @audit-issue Remove unused variable `minor` to save gas
  96 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {
```
GitHub: [96](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L96), [96](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L96)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Remove unused variable `major` to save gas
 100 |  function VERSION() external pure virtual returns (uint8 major, uint8 minor) {}

     |  // @audit-issue Remove unused variable `minor` to save gas
 100 |  function VERSION() external pure virtual returns (uint8 major, uint8 minor) {}

     |  // @audit-issue Remove unused variable `dependencies` to save gas
 152 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue Remove unused variable `requests` to save gas
 171 |  returns (Permissions[] memory requests)
```
GitHub: [100](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L100), [100](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L100), [152](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L152), [171](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L171)


## [G-19] Mutable state variables explicitly initialized to zero consume gas unnecessarily

The default value for variables is zero, so initializing them to zero is superfluous. This will also save gas if the variable is a mutable state variable.

*Instances: 18*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Initial value is unnecessary
 231 |  for (uint256 i = 0; i < items.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 341 |  for (uint256 i = 0; i < orders.length; ++i) {
```
GitHub: [231](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L231), [341](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L341)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Initial value is unnecessary
 197 |  for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 229 |  for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 249 |  for (uint256 i = 0; i < orderHashes.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 260 |  for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
```
GitHub: [197](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L197), [229](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L229), [249](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L249), [260](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L260)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Initial value is unnecessary
 120 |  for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
```
GitHub: [120](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L120)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Initial value is unnecessary
  90 |  for (uint256 i = 0; i < itemCount; ++i) {
```
GitHub: [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L90)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Initial value is unnecessary
 170 |  for (uint256 i = 0; i < order.items.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 176 |  for (uint256 i = 0; i < order.hooks.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 225 |  for (uint256 i = 0; i < metadata.hooks.length; ++i) {
```
GitHub: [170](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L170), [176](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L176), [225](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L225)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Initial value is unnecessary
 475 |  for (uint256 i = 0; i < hooks.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 599 |  for (uint256 i = 0; i < items.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 695 |  for (uint256 i = 0; i < executions.length; ++i) {
```
GitHub: [475](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L475), [599](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L599), [695](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L695)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Initial value is unnecessary
 205 |  for (uint256 i = 0; i < hooks.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 324 |  for (uint256 i = 0; i < orders.length; ++i) {

     |  // @audit-issue Initial value is unnecessary
 333 |  for (uint256 j = 0; j < orders[i].items.length; ++j) {
```
GitHub: [205](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205), [324](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L324), [333](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L333)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Initial value is unnecessary
 566 |  for (uint256 i = 0; i < reqLength; ++i) {
```
GitHub: [566](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L566)
