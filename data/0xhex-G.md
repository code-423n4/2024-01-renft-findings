# Gas Optimization

# Summary

| Number | Gas Optimization                                                   | Context |
| :----: | :----------------------------------------------------------------- | :-----: |
| [G-01] | Optimize Loop Counting Direction                                   |   29    |
| [G-02] | Use assembly to perform efficient back-to-back calls               |    2    |
| [G-03] | Improve Gas Efficiency in Struct Value Assignment                  |    6    |
| [G-04] | Using XOR (^) and AND (&) bitwise equivalents                      |    2    |
| [G-05] | Using storage instead of memory for structs/arrays saves gas       |    1    |
| [G-06] | Prefer Positive Conditional Flow for Efficiency                    |    1    |
| [G-07] | Consolidating Multiple Address/ID Mappings into a Single Mapping   |    5    |
| [G-08] | Gas-Efficient Alternatives for Common Math Operations              |    2    |
| [G-09] | Prefer Pre-increment and Pre-decrement Over +1 and -1 Operations   |    2    |
| [G-10] | Prefer bytes32 Over bytes or string for Constants                  |    2    |
| [G-11] | Prefer Do while Loops Over for Loops                               |   29    |
| [G-12] | Consider using OZ EnumerateSet in place of nested mappings         |    4    |
| [G-13] | Enhancing Memory Efficiency in External Calls with Inline Assembly |    4    |
| [G-14] | Use ERC721A instead ERC721                                         |    1    |
| [G-15] | Avoid Unnecessary Storage Updates                                  |    1    |
| [G-16] | Avoiding Contract Existence Checks with Low-Level Calls            |    6    |

## [G-01] Optimize Loop Counting Direction

In Solidity smart contracts, the direction of loop counting can impact gas efficiency. Counting down in `for` statements is more gas-efficient than counting up. This is because transitioning from a non-zero variable to zero in the last transaction of the loop provides a gas refund. The optimization involves reversing the loop counting direction to maximize gas efficiency.

```solidity
File: Kernel.sol

438 : for (uint256 i; i < depLength; ++i) {

512 : for (uint256 i; i < keycodeLen; ++i) {

521 : for (uint256 j; j < policiesLen; ++j) {

438 : for (uint256 i; i < depLength; ++i) {

566 : for (uint256 i = 0; i < reqLength; ++i) {

592 : for (uint256 i; i < depcLength; ++i) {
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L438

```solidity
File: PaymentEscrow.sol

231 : for (uint256 i = 0; i < items.length; ++i) {

341 : for (uint256 i = 0; i < orders.length; ++i) {
```

```solidity
File: Storage.sol


197 : for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {

249 : for (uint256 i = 0; i < orderHashes.length; ++i) {

197 : for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
```

```solidity
File: Accumulator.sol

120 : for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
```

```solidity
File: Reclaimer.sol
90 : for (uint256 i = 0; i < itemCount; ++i) {
```

```solidity
File: Signer.sol

170 : for (uint256 i = 0; i < order.items.length; ++i) {


176 : for (uint256 i = 0; i < order.hooks.length; ++i) {


225 : for (uint256 i = 0; i < metadata.hooks.length; ++i) {
```

```solidity
File: Create.sol

209 : for (uint256 i; i < offers.length; ++i) {


209 : for (uint256 i; i < offers.length; ++i) {

337 : for (uint256 i; i < considerations.length; ++i) {


337 : for (uint256 i; i < considerations.length; ++i) {


475 : for (uint256 i = 0; i < hooks.length; ++i) {


567 : for (uint256 i; i < items.length; ++i) {

599 : for (uint256 i = 0; i < items.length; ++i) {

695 : for (uint256 i = 0; i < executions.length; ++i) {
```

```solidity
File: src/policies/Stop.sol

205 : for (uint256 i = 0; i < hooks.length; ++i) {

276 : for (uint256 i; i < order.items.length; ++i) {

324 : for (uint256 i = 0; i < orders.length; ++i) {

333 : for (uint256 j = 0; j < orders[i].items.length; ++j) {
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205

## [G-02] Use assembly to perform efficient back-to-back calls

If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory expansion costs.
Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.
[Reffrence](https://code4rena.com/reports/2023-05-ajna#g-10-use-assembly-to-perform-efficient-back-to-back-calls)

```solidity
File:  src/policies/Factory.sol

124   ISafe(address(this)).enableModule(_stopPolicy);
127   ISafe(address(this)).setGuard(_guardPolicy);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124-L127

## [G-03] Improve Gas Efficiency in Struct Value Assignment

Gas savings can be achieved by reconsidering the model for assigning values to a structure. The conventional method, `structName a = structName({item1: val1, item2: val2});`, incurs higher gas costs compared to an alternative approach that separates the assignment of individual structure members.

```solidity
File:  src/policies/Create.sol

228         rentalItems[i + startIndex] = Item({
                itemType: itemType,
                settleTo: SettleTo.LENDER,
                token: offer.token,
                amount: offer.amount,
                identifier: offer.identifier
            });


331         rentalItems[i + startIndex] = Item({
                itemType: itemType,
                settleTo: settleTo,
                token: offer.token,
                amount: offer.amount,
                identifier: offer.identifier
            });


350         rentalItems[i + startIndex] = Item({
                itemType: ItemType.ERC20,
                settleTo: SettleTo.LENDER,
                token: consideration.token,
                amount: consideration.amount,
                identifier: consideration.identifier
            });

579         RentalOrder memory order = RentalOrder({
                seaportOrderHash: seaportPayload.orderHash,
                items: items,
                hooks: payload.metadata.hooks,
                orderType: payload.metadata.orderType,
                lender: seaportPayload.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + payload.metadata.rentDuration
            });

743      SeaportPayload memory seaportPayload = SeaportPayload({
            orderHash: zoneParams.orderHash,
            zoneHash: zoneParams.zoneHash,
            offer: zoneParams.offer,
            consideration: zoneParams.consideration,
            totalExecutions: zoneParams.totalExecutions,
            fulfiller: zoneParams.fulfiller,
            offerer: zoneParams.offerer
        });
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L228

## [G-04] Using XOR (^) and AND (&) bitwise equivalents

Given 4 variables a, b, c and d represented as such:

```
0 0 0 0 0 1 1 0 <- a
0 1 1 0 0 1 1 0 <- b
0 0 0 0 0 0 0 0 <- c
1 1 1 1 1 1 1 1 <- d

```

To have a == b means that every 0 and 1 match on both variables. Meaning that a XOR (operator ^) would evaluate to 0 ((a ^ b) == 0), as it excludes by definition any equalities.Now, if a != b, this means that there‚Äôs at least somewhere a 1 and a 0 not matching between a and b, making (a ^ b) != 0.Both formulas are logically equivalent and using the XOR bitwise operator costs actually the same amount of gas.However, it is much cheaper to use the bitwise OR operator (|) than comparing the truthy or falsy values.These are logically equivalent too, as the OR bitwise operator (|) would result in a 1 somewhere if any value is not 0 between the XOR (^) statements, meaning if any XOR (^) statement verifies that its arguments are different.

```solidity
File:  src/policies/Create.sol

311   if (totalRentals == 0 || totalPayments == 0) {

396   if (totalRentals == 0 || totalPayments == 0) {
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L311

## [G-05] Using storage instead of memory for structs/arrays saves gas

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

```solidity
File:  src/policies/Create.sol

579          RentalOrder memory order = RentalOrder({  // <= FOUND
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L579-L589

## [G-06] Prefer Positive Conditional Flow for Efficiency

In Solidity, utilizing a positive conditional flow is more gas-efficient than a negative one. The negation (`!`) operator in a negative condition introduces an additional computational cost in the form of a NOT opcode. Optimizing the conditional flow by placing the positive condition first can lead to gas savings by avoiding the need for the NOT opcode.

```solidity
File:  src/modules/Storage.sol

251            if (!orders[orderHashes[i]]) {
                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
            } else {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            }
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L251-L256

## [G-07] Consolidating Multiple Address/ID Mappings into a Single Mapping

In certain scenarios, consolidating multiple address/ID mappings into a single mapping of an address/ID to a struct can result in significant gas savings. This optimization not only saves a storage slot for each mapping combined but also has the potential to avoid a costly gas cost associated with Gsset (20,000 gas) per mapping combined. Additionally, reads and subsequent writes may become more economical when a function requires both values and they fit into the same storage slot. Furthermore, if both fields are accessed within the same function, there is an opportunity to save approximately 42 gas per access by eliminating the need to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and its associated stack operations.

```solidity
File:  src/modules/Storage.sol

33  mapping(address safe => uint256 nonce) public deployedSafes;   // <= FOUND

    uint256 public totalSafes;

    mapping(address to => address hook) internal _contractToHook;   // <= FOUND

    mapping(address hook => uint8 enabled) public hookStatus;     // <= FOUND

    mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;   // <= FOUND

    mapping(address extension => bool isWhitelisted) public whitelistedExtensions;   // <= FOUND
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L33-L58

## [G-08] Gas-Efficient Alternatives for Common Math Operations

Common math operations, such as min and max, may have more gas-efficient alternatives. In scenarios where unoptimized code uses conditional operators, like the ternary operator, the resulting conditional jumps in opcodes can incur higher gas costs. Exploring gas-efficient alternatives for these operations is crucial for optimizing smart contract performance.

```solidity
File:  src/modules/PaymentEscrow.sol

198    address settleToAddress = settleTo == SettleTo.LENDER ? lender : renter;
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L198

```solidity
File:  src/modules/Storage.sol

141  return hookStatus[hook] != 0 ? hook : address(0);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L141

## [G-09] Prefer Pre-increment and Pre-decrement Over +1 and -1 Operations

Gas savings can be achieved by favoring pre-increment and pre-decrement operations over the use of +1 and -1. Pre-increment (++i) and pre-decrement (--i) operations generally result in lower gas costs compared to their post-increment (i++) and post-decrement (i--) counterparts.

```solidity
File:  src/modules/Storage.sol

276   uint256 newSafeCount = totalSafes + 1;
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L276

```solidity
File:  src/policies/Factory.sol

184  uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L184

## [G-10] Prefer bytes32 Over bytes or string for Constants

When dealing with constants in Solidity, choosing the appropriate data type can impact gas efficiency. Using bytes32 is more efficient than using bytes or string for constants, especially when the data can fit within 32 bytes. The bytes32 data type is less robust in terms of flexibility compared to bytes or string but is highly gas-efficient for small, fixed-size data.

There are 2 instance(s) of this issue:

```solidity
File:  src/packages/Signer.sol

25      string internal constant _NAME = "ReNFT-Rentals";
26      string internal constant _VERSION = "1.0.0";
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L25-L26

## [G-11] Prefer Do while Loops Over for Loops

When optimizing gas usage in Solidity smart contracts, it is beneficial to consider using `do while` loops instead of `for` loops. The primary advantage lies in the fact that a `do while` loop incurs less gas cost since the loop condition is not checked during the initial iteration.

```solidity
File: Kernel.sol

438 : for (uint256 i; i < depLength; ++i) {

512 : for (uint256 i; i < keycodeLen; ++i) {

521 : for (uint256 j; j < policiesLen; ++j) {

438 : for (uint256 i; i < depLength; ++i) {

566 : for (uint256 i = 0; i < reqLength; ++i) {

592 : for (uint256 i; i < depcLength; ++i) {
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L438

```solidity
File: PaymentEscrow.sol

231 : for (uint256 i = 0; i < items.length; ++i) {

341 : for (uint256 i = 0; i < orders.length; ++i) {
```

```solidity
File: Storage.sol


197 : for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {

249 : for (uint256 i = 0; i < orderHashes.length; ++i) {

197 : for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
```

```solidity
File: Accumulator.sol

120 : for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
```

```solidity
File: Reclaimer.sol

90 : for (uint256 i = 0; i < itemCount; ++i) {
```

```solidity
File: Signer.sol

170 : for (uint256 i = 0; i < order.items.length; ++i) {


176 : for (uint256 i = 0; i < order.hooks.length; ++i) {


225 : for (uint256 i = 0; i < metadata.hooks.length; ++i) {
```

```solidity
File: Create.sol

209 : for (uint256 i; i < offers.length; ++i) {

337 : for (uint256 i; i < considerations.length; ++i) {

475 : for (uint256 i = 0; i < hooks.length; ++i) {

567 : for (uint256 i; i < items.length; ++i) {

599 : for (uint256 i = 0; i < items.length; ++i) {

695 : for (uint256 i = 0; i < executions.length; ++i) {
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L209

```solidity
File: src/policies/Stop.sol

205 : for (uint256 i = 0; i < hooks.length; ++i) {

276 : for (uint256 i; i < order.items.length; ++i) {

324 : for (uint256 i = 0; i < orders.length; ++i) {

333 : for (uint256 j = 0; j < orders[i].items.length; ++j) {
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205

## [G-12] Consider using OZ EnumerateSet in place of nested mappings

```solidity
File:  src/Kernel.sol

218    mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

221    mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
        public modulePermissions; // for policy addr, check if they have permission to call the function in the module.

226    mapping(address => mapping(Role => bool)) public hasRole;

229    mapping(address => mapping(Role => bool)) public hasRole;
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L218

## [G-13] Enhancing Memory Efficiency in External Calls with Inline Assembly

Using interfaces for external contract calls in Solidity is a convenient practice, but it may lead to inefficiencies in terms of memory utilization. Each external call results in the creation of a new memory location to store the data being passed, incurring memory expansion costs. This can be particularly impactful for contracts making frequent external calls.

Inline assembly provides a solution for optimizing memory usage by reusing already allocated memory spaces or utilizing scratch space for smaller datasets. This approach leads to notable gas savings, making it advantageous for contracts with a high frequency of external calls. Additionally, inline assembly allows for crucial safety checks, such as verifying if the target address has code deployed using `extcodesize(addr)` before making the call. These safety checks mitigate risks associated with contract interactions.

```solidity
File:  src/policies/Factory.sol

124   ISafe(address(this)).enableModule(_stopPolicy);

127   ISafe(address(this)).setGuard(_guardPolicy);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124-L127

```solidity
File:  src/policies/Stop.sol

168  bool success = ISafe(order.rentalWallet).execTransactionFromModule(

227  IHook(target).onStop(
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L168

## [G-14] Use ERC721A instead ERC721

ERC721A standard, ERC721A is an improvement standard for ERC721 tokens. It was proposed by the Azuki team and used for developing their NFT collection. Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

[Reffrence](https://nextrope.com/erc721-vs-erc721a-2/)

There are 1 instance(s) of this issue:

```solidity
File:  src/packages/Reclaimer.sol

4   import {IERC721} from "@openzeppelin-contracts/token/ERC721/IERC721.sol";
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L4

## [G-15] Avoid Unnecessary Storage Updates

Manipulating storage in Solidity can be gas-intensive. An optimization opportunity exists by avoiding unnecessary storage updates when the new value is identical to the existing value. This optimization minimizes gas consumption by preventing a Gsreset (2900 gas) operation, albeit potentially incurring a Gcoldsload (2100 gas) or a Gwarmaccess (100 gas).

```solidity
File:  src/Kernel.sol

193   isActive = activate_;
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L193

## [G-16] Avoiding Contract Existence Checks with Low-Level Calls

During external function calls in Solidity, the compiler may introduce additional code, including `EXTCODESIZE` (100 gas), to verify the existence of the target contract. In recent Solidity versions, this check is omitted when the external call has a return value. However, for earlier versions or when explicit control is desired, using low-level calls can achieve a similar behavior without incurring the gas costs associated with contract existence checks.

```solidity
File:  src/packages/Reclaimer.sol

33  IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier);

43  IERC1155(item.token).safeTransferFrom(
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33

```solidity
File:  src/policies/Create.sol

497 IHook(target).onStart(
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L497

```solidity
File:  src/policies/Factory.sol

124   ISafe(address(this)).enableModule(_stopPolicy);

127   ISafe(address(this)).setGuard(_guardPolicy);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124-L127

```solidity
File:  src/policies/Guard.sol

167  try IHook(hook).onTransaction(safe, to, value, data) {} catch Error(
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L167