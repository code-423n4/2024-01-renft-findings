# Gas Optimization


- ### [[G-01] Lack of unchecked in loops](#g-01-lack-of-unchecked-in-loops)
- ### [[G-02] Calldata should be used in place of memory function parameters when not mutated](#g-02-calldata-should-be-used-in-place-of-memory-function-parameters-when-not-mutated)
- ### [[G-03] Expression ("") is cheaper than new bytes(0)](#g-03-expression--is-cheaper-than-new-bytes0)
- ### [[G-04] Use assembly hashing](#g-05-gas-optimization-using-assembly-to-write-address-storage-value)
- ### [[G-05] Gas Optimization: Using Assembly to Write Address Storage Value](#g-05-gas-optimization-using-assembly-to-write-address-storage-value)
- ### [[G-06] abi.encode() is less efficient than abi.encodePacked()](#g-06-abiencode-is-less-efficient-than-abiencodepacked)
- ### [[G-07] Gas Optimization: Consolidating Multiple Address/ID Mappings into a Single Mapping](#g-07-gas-optimization-consolidating-multiple-addressid-mappings-into-a-single-mapping)
- ### [[G-08] Using storage instead of memory for structs/arrays saves gas](#g-08-using-storage-instead-of-memory-for-structsarrays-saves-gas)
- ### [[G-09] Use ERC721A instead ERC721](#g-09-use-erc721a-instead-erc721)
- ### [[G-10] Gas Optimization: Prefer bytes32 Over bytes or string for Constants](#g-10-gas-optimization-prefer-bytes32-over-bytes-or-string-for-constants)
- ### [[G-11] Gas Optimization Issue - Gas-Efficient Alternatives for Common Math Operations](#g-11-gas-optimization-issue---gas-efficient-alternatives-for-common-math-operations)
- ### [[G-12] Gas Optimization: Prefer Positive Conditional Flow for Efficiency](#g-12-gas-optimization-prefer-positive-conditional-flow-for-efficiency)
- ### [[G-13] Gas Optimization: Enhancing Memory Efficiency in External Calls with Inline Assembly](#g-13-gas-optimization-enhancing-memory-efficiency-in-external-calls-with-inline-assembly)
- ### [[G-14] Gas Optimization: Avoiding Contract Existence Checks with Low-Level Calls](#g-14-gas-optimization-avoiding-contract-existence-checks-with-low-level-calls)
- ### [[G-15] Gas Optimization: Optimize Loop Counting Direction](#g-15-gas-optimization-optimize-loop-counting-direction)
- ### [[G-16] Gas Optimization: Prefer `do while` Loops Over `for` Loops](#g-16-gas-optimization-prefer-do-while-loops-over-for-loops)
- ### [[G-17] Gas Optimization: Improve Gas Efficiency in Struct Value Assignment](#g-17-gas-optimization-improve-gas-efficiency-in-struct-value-assignment)
- ### [[G-18] Gas Optimization: Prefer Pre-increment and Pre-decrement Over +1 and -1 Operations](#g-18-gas-optimization-prefer-pre-increment-and-pre-decrement-over-1-and--1-operations)
- ### [[G-19] Gas Optimization: Avoid Unnecessary Storage Updates](#g-19-gas-optimization-avoid-unnecessary-storage-updates)
- ### [[G-20] Consider using OZ EnumerateSet in place of nested mappings](#g-20-consider-using-oz-enumerateset-in-place-of-nested-mappings)
- ### [[G-21] Using XOR (^) and AND (&) bitwise equivalents](#g-10-gas-optimization-prefer-bytes32-over-bytes-or-string-for-constants)
- ### [[G-22] Use assembly to perform efficient back-to-back calls](#g-22-use-assembly-to-perform-efficient-back-to-back-calls)
- ### [[G-23] Gas Optimization: Use `uint256(1)`/`uint256(2)` Instead for True and False Boolean States](#g-23-gas-optimization-use-uint2561uint2562-instead-for-true-and-false-boolean-states)

## [G-01] Lack of unchecked in loops

 #### `note` : All instances are not found by bot race

**Explanation of Issue:**
In Solidity, the `unchecked` keyword can be used to skip overflow checks on arithmetic operations. By default, Solidity performs overflow checks on increment (`++`) and decrement (`--`) operations. However, if it is guaranteed that overflows won't occur within a loop, using `unchecked` at the end of the `for` loop body with `i++` can make the loop more gas-efficient by avoiding unnecessary checks.

**Examples of Code:**
- *Non-optimized Code:*
```solidity
for (uint256 i = 0; i < 10; i++) {
    // Loop logic
}
```

- *Optimized Code:*
```solidity
for (uint256 i = 0; i < 10;) {
    // Loop logic
    unchecked {
        i++;
    }
}
```

**Reason:**  
The lack of `unchecked` at the end of the `for` loop body for the increment operation (`i++`) can result in extra gas consumption due to the overhead of performing overflow checks. If it is certain that overflows won't occur within the loop, using `unchecked` allows the compiler to skip these checks, resulting in improved gas efficiency. As a Solidity smart contract auditor and gas optimizer, it is important to identify opportunities for gas optimization like using `unchecked` in loops to reduce gas costs and enhance the overall efficiency of the smart contract.

There are 10 instance(s) of this issue:

```solidity
File:  src/modules/Storage.sol
229     for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L229-L234


```solidity
File:  src/modules/Storage.sol
260     for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L260-L265


```solidity
File:  src/packages/Signer.sol
176     for (uint256 i = 0; i < order.hooks.length; ++i) {
            // Hash the hook.
            hookHashes[i] = _deriveHookHash(order.hooks[i]);
        }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L176-L179


```solidity
File:  src/policies/Create.sol
209  for (uint256 i; i < offers.length; ++i) {

337  for (uint256 i; i < considerations.length; ++i) {   

567  for (uint256 i; i < items.length; ++i) {

599  for (uint256 i = 0; i < items.length; ++i) {         
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L209


```solidity
File:  src/Kernel.sol
521  for (uint256 j; j < policiesLen; ++j) {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L521


```solidity
File:  src/packages/Accumulator.sol
120   for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L120


```solidity
File:  src/packages/Reclaimer.sol
90   for (uint256 i = 0; i < itemCount; ++i) {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L90



## [G-02] Calldata should be used in place of memory function parameters when not mutated
 #### `note` : All instances are not found by bot race
**Explanation of Issue:**
In Solidity, when an external function does not modify function parameters of type `bytes memory`, `string memory`, or `[] memory`, it is recommended to use `calldata` instead of `memory` for these parameters. `calldata` is a non-modifiable, non-persistent area where function arguments are stored, and it is generally cheaper in terms of gas compared to `memory`. This optimization is particularly beneficial for arrays and complex data types when used in external function calls. 

**Examples of Code:**
- *Non-optimized Code:*
```solidity
function processArray(uint256[] memory data) external {
    // Function logic using the data array
}
```

- *Optimized Code:*
```solidity
function processArray(uint256[] calldata data) external {
    // Function logic using the data array
}
```

**Reason:**  
Using `calldata` instead of `memory` for function parameters in external functions saves gas by avoiding the need for copying data to `memory`. This approach provides direct read-only access to function arguments from the input data, resulting in reduced gas consumption. It is particularly beneficial for large arrays or complex data types, as it eliminates the gas cost associated with copying data to `memory`. This optimization improves contract efficiency, reduces costs for users, and enhances overall performance.

There are 5 instance(s) of this issue:

```solidity
File:  src/Create2Deployer.sol
32  function deploy(
        bytes32 salt,
        bytes memory initCode   // <= FOUND
    ) external payable returns (address deploymentAddress) {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L32-L35


```solidity
File:  src/policies/Create.sol
126 function getRentalOrderHash(
        RentalOrder memory order  // <= FOUND
    ) external view returns (bytes32) {

137 function getRentPayloadHash(
        RentPayload memory payload  // <= FOUND
    ) external view returns (bytes32) {  

148 function getOrderMetadataHash(
        OrderMetadata memory metadata
    ) external view returns (bytes32) {              
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L126-L128


```solidity
File:  src/policies/Guard.sol
309 function checkTransaction(
        address to,
        uint256 value,
        bytes memory data,
        Enum.Operation operation,
        uint256,
        uint256,
        uint256,
        address,
        address payable,
        bytes memory,
        address
    ) external override {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L309-L321



## [G-03] Expression ("") is cheaper than new bytes(0)
 #### `note` : All instances are not found by bot race
**Explanation of Issue:**
In Solidity, using an empty string `""` instead of `new bytes(0)` in expressions can result in reduced gas costs. When `new bytes(0)` is used, it creates a dynamic byte array, which incurs additional gas overhead. However, by simply using `""` when an empty bytes array is required, you can optimize gas efficiency.

**Examples of Code:**
- *Non-optimized Code:*
```solidity
bytes memory emptyBytes = new bytes(0);
```

- *Optimized Code:*
```solidity
bytes memory emptyBytes = "";
```

**Reason:**  
The issue arises when `new bytes(0)` is used to create an empty bytes array. This approach incurs extra gas overhead because it dynamically allocates memory for a bytes array of size zero. On the other hand, using `""` directly assigns an empty string literal to the bytes variable, resulting in cheaper gas costs.

There are 2 instance(s) of this issue:
```solidity
File:  src/policies/Stop.sol
272   bytes memory rentalAssetUpdates = new bytes(0);

320   bytes memory rentalAssetUpdates = new bytes(0);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L272

## [G-04] Use assembly hashing
 #### `note` : All instances are not found by bot race
**Explanation of Issue:**
When it comes to gas efficiency, using the assembly version of the `keccak256` hashing function can be more advantageous than using the high-level Solidity version. This is because Solidity incurs additional gas costs due to function call overhead and memory management when executing the hashing function.

**Examples of Code:**
- *Non-optimized Code:*
```solidity
bytes32 hash = keccak256(abi.encodePacked(data));
```

- *Optimized Code:*
```solidity
bytes32 hash;
assembly {
    hash := keccak256(add(data, 32), mload(data))
}
```

**Reason:**  
The issue arises when the high-level Solidity version of `keccak256` is used to compute the hash. This version involves function call overhead and memory management, which can increase the gas cost.
By utilizing the assembly version of `keccak256`, you can bypass the additional overhead and directly invoke the hashing function. This approach involves loading the data into memory and using assembly instructions to compute the hash. As a result, the gas cost is reduced compared to the Solidity version.


There are 8 instance(s) of this issue:
```solidity
File: src/packages/Signer.sol
128          keccak256(
                abi.encode(
                    _ITEM_TYPEHASH,
                    item.itemType,
                    item.settleTo,
                    item.token,
                    item.amount,
                    item.identifier
                )
            );

150         keccak256(
                abi.encode(_HOOK_TYPEHASH, hook.target, hook.itemIndex, hook.extraData)
            );  

182         keccak256(
                abi.encode(
                    _RENTAL_ORDER_TYPEHASH,
                    order.seaportOrderHash,
                    keccak256(abi.encodePacked(itemHashes)),
                    keccak256(abi.encodePacked(hookHashes)),
                    order.orderType,
                    order.lender,
                    order.renter,
                    order.startTimestamp,
                    order.endTimestamp
                )
            );  

208    return keccak256(abi.encode(_ORDER_FULFILLMENT_TYPEHASH, fulfillment.recipient)); 


232         keccak256(
                abi.encode(
                    _ORDER_METADATA_TYPEHASH,
                    metadata.rentDuration,
                    keccak256(abi.encodePacked(hookHashes))
                )
            );


253         keccak256(
                abi.encode(
                    _RENT_PAYLOAD_TYPEHASH,
                    _deriveOrderFulfillmentHash(payload.fulfillment),
                    _deriveOrderMetadataHash(payload.metadata),
                    payload.expiration,
                    payload.intendedFulfiller
                )
            );    


279         keccak256(
                abi.encode(
                    _eip712DomainTypeHash,
                    _nameHash,
                    _versionHash,
                    block.chainid,
                    address(this)
                )
            );                    
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L128


```solidity
File:  src/policies/Factory.sol
184    uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L184


## [G-05] Gas Optimization: Using Assembly to Write Address Storage Value

**Explanation of Issue:**
When writing to address storage values in Solidity, standard operations can result in higher gas costs. Utilizing assembly code provides an opportunity to optimize gas usage by directly interacting with the Ethereum Virtual Machine (EVM) and executing low-level operations that are not achievable with Solidity alone.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    contract MyContract {
        address private myAddress;
        
        function setAddressUsingSolidity(address newAddress) public {
            myAddress = newAddress;
        }
    }
    ```
  - *Optimized Code:*
    ```solidity
    contract MyContract {
        address private myAddress;
        
        function setAddressUsingAssembly(address newAddress) public {
            assembly {
                sstore(0, newAddress)
            }
        }
    }
    ```

**Reason:**  
Using Solidity to write to address storage involves standard operations that may result in higher gas consumption. The optimized code demonstrates the use of assembly to directly invoke the `sstore` operation, bypassing unnecessary operations and reducing gas costs associated with writing to storage. This approach leverages the flexibility of assembly to achieve a more gas-efficient implementation in smart contracts.

There are 4 instance(s) of this issue:

```solidity
File:  src/Kernel.sol
243         executor = _executor;
244         admin = _admin;

296  executor = target_;

298  admin = target_;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L243-L244


## [G-06] abi.encode() is less efficient than abi.encodePacked()
In terms of efficiency, abi.encodePacked() is generally considered to be more gas-efficient than abi.encode(), because it skips the step of adding function signatures and other metadata to the encoded data. However, this comes at the cost of reduced safety, as abi.encodePacked() does not perform any type checking or padding of data.

There are 5 instances of this issue:

```solidity
File:   src/packages/Signer.sol
208   return keccak256(abi.encode(_ORDER_FULFILLMENT_TYPEHASH, fulfillment.recipient));

233             abi.encode(
                    _ORDER_METADATA_TYPEHASH,
                    metadata.rentDuration,
                    keccak256(abi.encodePacked(hookHashes))
                )


254             abi.encode(
                    _RENT_PAYLOAD_TYPEHASH,
                    _deriveOrderFulfillmentHash(payload.fulfillment),
                    _deriveOrderMetadataHash(payload.metadata),
                    payload.expiration,
                    payload.intendedFulfiller
                )

280             abi.encode(
                    _eip712DomainTypeHash,
                    _nameHash,
                    _versionHash,
                    block.chainid,
                    address(this)
                )      


374             abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)                          
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L208



## [G-07] Gas Optimization: Consolidating Multiple Address/ID Mappings into a Single Mapping

**Explanation of Issue:**
In certain scenarios, consolidating multiple address/ID mappings into a single mapping of an address/ID to a struct can result in significant gas savings. This optimization not only saves a storage slot for each mapping combined but also has the potential to avoid a costly gas cost associated with Gsset (20,000 gas) per mapping combined. Additionally, reads and subsequent writes may become more economical when a function requires both values and they fit into the same storage slot. Furthermore, if both fields are accessed within the same function, there is an opportunity to save approximately 42 gas per access by eliminating the need to recalculate the key's keccak256 hash (Gkeccak256 - 30 gas) and its associated stack operations.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    contract NonOptimizedContract {
        mapping(address => uint256) idMapping1;
        mapping(address => uint256) idMapping2;
        
        function getValue1(address user) public view returns (uint256) {
            return idMapping1[user];
        }
        
        function getValue2(address user) public view returns (uint256) {
            return idMapping2[user];
        }
    }
    ```
  - *Optimized Code:*
    ```solidity
    contract OptimizedContract {
        struct UserStruct {
            uint256 idMapping1;
            uint256 idMapping2;
        }
        
        mapping(address => UserStruct) userMappings;
        
        function getValues(address user) public view returns (uint256, uint256) {
            return (userMappings[user].idMapping1, userMappings[user].idMapping2);
        }
    }
    ```

**Reason:**  
The non-optimized code demonstrates the use of separate mappings for different fields related to an address. The optimized code consolidates these mappings into a single mapping of an address to a struct, reducing the number of storage slots used and potentially avoiding the gas cost associated with multiple Gsset operations. This optimization enhances gas efficiency in storage operations, making reads and writes more economical, especially when both values are required within the same function. Additionally, it reduces gas consumption by eliminating the need to recalculate the keccak256 hash for each access in the same function.


There are 1 instance(s) of this issue:
```solidity
File:  src/modules/Storage.sol
33 mapping(address safe => uint256 nonce) public deployedSafes;   // <= FOUND

    // Records the total amount of deployed safes.
    uint256 public totalSafes;

    /////////////////////////////////////////////////////////////////////////////////
    //                                 Hook Storage                                //
    /////////////////////////////////////////////////////////////////////////////////

    // When interacting with the guard, any contracts that have hooks enabled
    // should have the guard logic routed through them.
    mapping(address to => address hook) internal _contractToHook;   // <= FOUND

    // Mapping of a bitmap which denotes the hook functions that are enabled.
    mapping(address hook => uint8 enabled) public hookStatus;     // <= FOUND

    /////////////////////////////////////////////////////////////////////////////////
    //                            Whitelist Storage                                //
    /////////////////////////////////////////////////////////////////////////////////

    // Allows the safe to delegate call to an approved address. For example, delegate
    // call to a contract that would swap out an old gnosis safe module for a new one.
    mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;   // <= FOUND

    // Allows for the safe registration of extensions that can be enabled on a safe.
    mapping(address extension => bool isWhitelisted) public whitelistedExtensions;   // <= FOUND
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L33-L58


- like this 
```solidity
    struct SafeData {
        uint256 nonce;
        address hook;
        uint8 hookStatus;
        bool isWhitelistedDelegate;
        bool isWhitelistedExtension;
    }

    mapping(address => SafeData) public safeData;
    uint256 public totalSafes;
```

## [G-08] Using storage instead of memory for structs/arrays saves gas
When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

There are 1 instance(s) of this issue:
```solidity
File:  src/policies/Create.sol
579          RentalOrder memory order = RentalOrder({  // <= FOUND
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
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L579-L589


## [G-09] Use ERC721A instead ERC721
ERC721A standard, ERC721A is an improvement standard for ERC721 tokens. It was proposed by the Azuki team and used for developing their NFT collection. Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

[Reffrence](https://nextrope.com/erc721-vs-erc721a-2/)

There are 1 instance(s) of this issue:
```solidity
File:  src/packages/Reclaimer.sol
4   import {IERC721} from "@openzeppelin-contracts/token/ERC721/IERC721.sol";
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L4

## [G-10] Gas Optimization: Prefer bytes32 Over bytes or string for Constants

**Explanation of Issue:**
When dealing with constants in Solidity, choosing the appropriate data type can impact gas efficiency. Using bytes32 is more efficient than using bytes or string for constants, especially when the data can fit within 32 bytes. The bytes32 data type is less robust in terms of flexibility compared to bytes or string but is highly gas-efficient for small, fixed-size data.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    contract NonOptimizedContract {
        string public constant STRING_CONSTANT = "Hello, World!";
    }
    ```
  - *Optimized Code:*
    ```solidity
    contract OptimizedContract {
        bytes32 public constant BYTES32_CONSTANT = 0x48656c6c6f2c20576f726c6421;
    }
    ```

**Reason:**  
In the non-optimized code, a string constant is used, which is less gas-efficient compared to the optimized code that uses bytes32. The optimized code represents the constant "Hello, World!" in hexadecimal format, fitting it within 32 bytes. This reduces gas consumption, as bytes32 operations are generally more efficient for constants that fit within this size limit. While bytes32 sacrifices some flexibility compared to bytes or string, the optimization is warranted for gas efficiency when the data can be accommodated within 32 bytes. This approach is especially beneficial in scenarios where minimizing gas costs is a priority.

There are 2 instance(s) of this issue:
```solidity
File:  src/packages/Signer.sol
25      string internal constant _NAME = "ReNFT-Rentals";
26      string internal constant _VERSION = "1.0.0";
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L25-L26

## [G-11] Gas Optimization Issue - Gas-Efficient Alternatives for Common Math Operations

**Explanation of Issue:**
Common math operations, such as min and max, may have more gas-efficient alternatives. In scenarios where unoptimized code uses conditional operators, like the ternary operator, the resulting conditional jumps in opcodes can incur higher gas costs. Exploring gas-efficient alternatives for these operations is crucial for optimizing smart contract performance.

**Examples of Code:**
*Non-optimized Code:*
```solidity
function max(uint256 x, uint256 y) public pure returns (uint256 z) {
    z = x > y ? x : y; // Unoptimized conditional operator
}
```

*Optimized Code:*
```solidity
function max(uint256 x, uint256 y) public pure returns (uint256 z) {
    /// @solidity memory-safe-assembly
    assembly {
        z := xor(x, mul(xor(x, y), gt(y, x))) // Gas-efficient alternative
    }
}
```

**Reason:**
In the non-optimized example, the ternary operator is used for the max function, introducing conditional jumps in the opcodes, which can be gas-costly. The optimized code provides a gas-efficient alternative using assembly code. By avoiding conditional operators, the gas consumption is reduced, leading to improved contract efficiency. The Solady Library offers additional gas-efficient math operations, making it valuable for developers seeking optimization opportunities.

There are 2 instance(s) of this issue:
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


## [G-12] Gas Optimization: Prefer Positive Conditional Flow for Efficiency

**Explanation of Issue:**
In Solidity, utilizing a positive conditional flow is more gas-efficient than a negative one. The negation (`!`) operator in a negative condition introduces an additional computational cost in the form of a NOT opcode. Optimizing the conditional flow by placing the positive condition first can lead to gas savings by avoiding the need for the NOT opcode.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    contract NonOptimizedContract {
        function checkNegativeCondition(bool cond) public pure returns (bool) {
            if (!cond) {
                // Code to execute when the condition is false
                return false;
            } else {
                // Code to execute when the condition is true
                return true;
            }
        }
    }
    ```
  - *Optimized Code:*
    ```solidity
    contract OptimizedContract {
        function checkPositiveCondition(bool cond) public pure returns (bool) {
            if (cond) {
                // Code to execute when the condition is true
                return true;
            } else {
                // Code to execute when the condition is false
                return false;
            }
        }
    }
    ```

**Reason:**  
The non-optimized code uses a negative condition (`!cond`), requiring a NOT opcode and incurring additional computational cost. The optimized code rearranges the conditional flow to use a positive condition (`cond`), eliminating the need for the NOT opcode and resulting in gas savings. This optimization is beneficial for scenarios where minimizing gas costs is crucial, as it reduces the computational overhead associated with negative conditions.

There are 1 instance(s) of this issue:
```solidity
File:  src/modules/Storage.sol
            if (!orders[orderHashes[i]]) {
                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
            } else {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L251-L256

Fix code:

```solidity
            if (orders[orderHashes[i]]) {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            } else {
                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
            }
```



## [G-13] Gas Optimization: Enhancing Memory Efficiency in External Calls with Inline Assembly

**Explanation of Issue:**
Using interfaces for external contract calls in Solidity is a convenient practice, but it may lead to inefficiencies in terms of memory utilization. Each external call results in the creation of a new memory location to store the data being passed, incurring memory expansion costs. This can be particularly impactful for contracts making frequent external calls.

Inline assembly provides a solution for optimizing memory usage by reusing already allocated memory spaces or utilizing scratch space for smaller datasets. This approach leads to notable gas savings, making it advantageous for contracts with a high frequency of external calls. Additionally, inline assembly allows for crucial safety checks, such as verifying if the target address has code deployed using `extcodesize(addr)` before making the call. These safety checks mitigate risks associated with contract interactions.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    contract NonOptimizedContract {
        ExternalContractInterface externalContract;

        function makeExternalCall(uint256 data) public {
            externalContract.externalFunction(data);
        }
    }
    ```
  - *Optimized Code:*
    ```solidity
    contract OptimizedContract {
        function makeExternalCallWithAssembly(address externalContract, uint256 data) public {
            // Safety check
            require(extcodesize(externalContract) > 0, "Target address has no code deployed");

            // Inline assembly for optimized memory usage
            assembly {
                // Use existing memory space or scratch space for data
                let freeMemoryPointer := mload(0x40)
                mstore(freeMemoryPointer, data)

                // Make external call
                let success := call(gas(), externalContract, 0, freeMemoryPointer, 32, 0, 0)

                // Check if the call was successful
                if iszero(success) {
                    revert(0, 0)
                }
            }
        }
    }
    ```

**Reason:**  
The non-optimized code uses an interface for an external contract call, leading to inefficient memory usage due to the creation of new memory locations. The optimized code utilizes inline assembly to optimize memory usage by reusing existing memory space or scratch space for data. This approach, along with safety checks, results in notable gas savings, especially for contracts making frequent external calls. The optimization enhances overall efficiency in memory utilization and mitigates risks associated with external contract interactions.


There are 4 instance(s) of this issue:
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



## [G-14] Gas Optimization: Avoiding Contract Existence Checks with Low-Level Calls

**Explanation of Issue:**
During external function calls in Solidity, the compiler may introduce additional code, including `EXTCODESIZE` (100 gas), to verify the existence of the target contract. In recent Solidity versions, this check is omitted when the external call has a return value. However, for earlier versions or when explicit control is desired, using low-level calls can achieve a similar behavior without incurring the gas costs associated with contract existence checks.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    contract NonOptimizedContract {
        ExternalContractInterface externalContract;

        function makeExternalCall(uint256 data) public {
            require(address(externalContract).code.length > 0, "Target contract does not exist");
            externalContract.externalFunction(data);
        }
    }
    ```
  - *Optimized Code:*
    ```solidity
    contract OptimizedContract {
        function makeExternalCallLowLevel(address externalContract, uint256 data) public {
            // Low-level call without explicit contract existence check
            bool success;
            bytes memory result;
            (success, result) = externalContract.call(abi.encodeWithSignature("externalFunction(uint256)", data));

            // Check if the call was successful
            require(success, "External call failed");
        }
    }
    ```

**Reason:**  
The non-optimized code explicitly checks for the existence of the target contract using `EXTCODESIZE`, introducing additional gas costs. The optimized code uses a low-level call without explicit contract existence checks, achieving a similar behavior while avoiding the gas expenses associated with these checks. This optimization is valuable in scenarios where minimizing gas costs is crucial, and the existence of the target contract can be assumed or verified through alternative means.

There are 6 instances of this issue:
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

## [G-15] Gas Optimization: Optimize Loop Counting Direction

**Explanation of Issue:**

In Solidity smart contracts, the direction of loop counting can impact gas efficiency. Counting down in `for` statements is more gas-efficient than counting up. This is because transitioning from a non-zero variable to zero in the last transaction of the loop provides a gas refund. The optimization involves reversing the loop counting direction to maximize gas efficiency.

**Examples of Code:**

- *Non-optimized Code:*
```solidity
// Example of non-optimized code counting up in a for loop
function processElements(uint256 limit) public {
    for (uint256 i = 0; i < limit; i++) {
        // Loop body - process elements
        // ...
    }
}
```

- *Optimized Code:*
```solidity
// Example of optimized code counting down in a for loop
function processElements(uint256 limit) public {
    for (uint256 i = limit; i > 0; i--) {
        // Loop body - process elements
        // ...
    }
}
```

**Reason:** 

The optimization involves reversing the direction of loop counting from counting up to counting down. Counting down in a `for` loop is more gas-efficient as it allows for a gas refund when transitioning from a non-zero variable to zero in the last transaction of the loop. This approach contributes to gas savings, making the smart contract execution more efficient in terms of gas consumption.


There are 29 instance(s) of this issue:
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
```

```solidity
File: Storage.sol
197 : for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
```

```solidity
File: Storage.sol
249 : for (uint256 i = 0; i < orderHashes.length; ++i) {
```

```solidity
File: Storage.sol
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
```

```solidity
File: Signer.sol
176 : for (uint256 i = 0; i < order.hooks.length; ++i) {
```

```solidity
File: Signer.sol
225 : for (uint256 i = 0; i < metadata.hooks.length; ++i) {
```

```solidity
File: Create.sol
209 : for (uint256 i; i < offers.length; ++i) {
```

```solidity
File: Create.sol
209 : for (uint256 i; i < offers.length; ++i) {
```

```solidity
File: Create.sol
337 : for (uint256 i; i < considerations.length; ++i) {
```

```solidity
File: Create.sol
337 : for (uint256 i; i < considerations.length; ++i) {
```

```solidity
File: Create.sol
475 : for (uint256 i = 0; i < hooks.length; ++i) {
```

```solidity
File: Create.sol
567 : for (uint256 i; i < items.length; ++i) {
```

```solidity
File: Create.sol
599 : for (uint256 i = 0; i < items.length; ++i) {
```

```solidity
File: Create.sol
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



## [G-16] Gas Optimization: Prefer `do while` Loops Over `for` Loops

**Explanation of Issue:**

When optimizing gas usage in Solidity smart contracts, it is beneficial to consider using `do while` loops instead of `for` loops. The primary advantage lies in the fact that a `do while` loop incurs less gas cost since the loop condition is not checked during the initial iteration.

**Examples of Code:**

- *Non-optimized Code:*
```solidity
// Example of non-optimized code using a for loop
function processElements() public {
    for (uint256 i = 0; i < 10; i++) {
        // Loop body - process elements
        // ...
    }
}
```

- *Optimized Code:*
```solidity
// Example of optimized code 1
        i++;
    } while (i < 10);
}
```

**Reason:** 

The optimization involves replacing `for` loops with `do while` loops to achieve reduced gas consumption. In a `do while` loop, the loop condition is only checked after the initial iteration. This eliminates the gas cost associated with checking the condition before the first iteration, making it a more gas-efficient choice. When processing a known number of iterations, such as in the provided example, switching to `do while` loops can contribute to overall gas savings in smart contract execution.



There are 1 instances of this issue:
```solidity
File:  src/policies/Create.sol
333   for (uint256 i; i < considerations.length; ++i) {
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L333


## [G-17] Gas Optimization: Improve Gas Efficiency in Struct Value Assignment

**Explanation of Issue:**
Gas savings can be achieved by reconsidering the model for assigning values to a structure. The conventional method, `structName a = structName({item1: val1, item2: val2});`, incurs higher gas costs compared to an alternative approach that separates the assignment of individual structure members.

**Examples of Code:**

- *Non-optimized Code:*
```solidity
// Example of non-optimized code with structure value assignment
struct MyStruct {
    uint256 item1;
    uint256 item2;
}

function assignStructValues(uint256 val1, uint256 val2) public returns (MyStruct) {
    MyStruct a = MyStruct({item1: val1, item2: val2});
    return a;
}
```

- *Optimized Code:*
```solidity
// Example of optimized code with separated structure member assignments
struct MyStruct {
    uint256 item1;
    uint256 item2;
}

function assignStructValues(uint256 val1, uint256 val2) public returns (MyStruct) {
    MyStruct a;
    a.item1 = val1;
    a.item2 = val2;
    return a;
}
```

**Reason:** 

The optimization involves changing the model for assigning values to a structure, moving from the traditional single-line initialization to a two-step process. The non-optimized code utilizes a single line to initialize the structure with values, which incurs higher gas costs. In contrast, the optimized code separates the assignment of individual structure members, resulting in reduced gas consumption during structure initialization. This modification contributes to overall gas efficiency in smart contract execution.

There are 5 instances of this issue:
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



## [G-18] Gas Optimization: Prefer Pre-increment and Pre-decrement Over +1 and -1 Operations

**Explanation of Issue:**

Gas savings can be achieved by favoring pre-increment and pre-decrement operations over the use of `+1` and `-1`. Pre-increment (`++i`) and pre-decrement (`--i`) operations generally result in lower gas costs compared to their post-increment (`i++`) and post-decrement (`i--`) counterparts.

**Examples of Code:**

- *Non-optimized Code:*
```solidity
// Example of non-optimized code using +1 and -1 operations
function incrementAndDecrement(uint256 value) public pure returns (uint256) {
    uint256 result1 = value + 1;
    uint256 result2 = value - 1;

    return result1 * result2;
}
```

- *Optimized Code:*
```solidity
// Example of optimized code using pre-increment and pre-decrement
function incrementAndDecrement(uint256 value) public pure returns (uint256) {
    uint256 result1 = ++value;
    uint256 result2 = --value;

    return result1 * result2;
}
```

**Reason:** 

The optimization involves replacing `+1` and `-1` operations with pre-increment (`++i`) and pre-decrement (`--i`) operations. In most cases, pre-increment and pre-decrement operations result in lower gas costs compared to their post-increment and post-decrement counterparts. This modification enhances gas efficiency, especially in scenarios where these operations are used within loops or other repetitive structures. It is a recommended practice for achieving better gas optimization in Solidity smart contracts.


There are 2 instances of this issue:
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

## [G-19] Gas Optimization: Avoid Unnecessary Storage Updates

**Explanation of Issue:**

Manipulating storage in Solidity can be gas-intensive. An optimization opportunity exists by avoiding unnecessary storage updates when the new value is identical to the existing value. This optimization minimizes gas consumption by preventing a Gsreset (2900 gas) operation, albeit potentially incurring a Gcoldsload (2100 gas) or a Gwarmaccess (100 gas).

**Examples of Code:**

- *Non-optimized Code:*
```solidity
// Example of non-optimized code with unnecessary storage updates
contract StorageManipulation {
    uint256 public storedValue;

    function updateStorage(uint256 newValue) public {
        // Unconditionally updating storage even if the new value is the same
        storedValue = newValue;
    }
}
```

- *Optimized Code:*
```solidity
// Example of optimized code avoiding unnecessary storage updates
contract StorageManipulation {
    uint256 public storedValue;

    function updateStorage(uint256 newValue) public {
        // Only update storage if the new value is different from the existing value
        if (storedValue != newValue) {
            storedValue = newValue;
        }
    }
}
```

**Reason:** 

The optimization involves introducing a check before updating storage to ensure that the new value differs from the existing value. By avoiding unnecessary storage updates when the values are identical, the gas-intensive Gsreset operation (2900 gas) can be skipped. While this may introduce a Gcoldsload (2100 gas) or a Gwarmaccess (100 gas), the overall gas savings outweigh the potential cost. This optimization strategy contributes to a more gas-efficient execution of smart contract operations.


```solidity
File:  src/Kernel.sol
193   isActive = activate_;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L193


## [G-20] Consider using OZ EnumerateSet in place of nested mappings
Nested mappings and multi-dimensional arrays in Solidity operate through a process of double hashing, wherein the original storage slot and the first key are concatenated and hashed, and then this hash is again concatenated with the second key and hashed. This process can be quite gas expensive due to the double-hashing operation and subsequent storage operation (sstore).
A possible optimization involves manually concatenating the keys followed by a single hash operation and an sstore. However, this technique introduces the risk of storage collision, especially when there are other nested hash maps in the contract that use the same key types. Because Solidity is unaware of the number and structure of nested hash maps in a contract, it follows a conservative approach in computing the storage slot to avoid possible collisions.
OpenZeppelin's EnumerableSet provides a potential solution to this problem. It creates a data structure that combines the benefits of set operations with the ability to enumerate stored elements, which is not natively available in Solidity. EnumerableSet handles the element uniqueness internally and can therefore provide a more gas-efficient and collision-resistant alternative to nested mappings or multi-dimensional arrays in certain scenarios.

There are 4 instance(s) of this issue:

```solidity
File:  src/Kernel.sol
218    mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

221    mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
        public modulePermissions; // for policy addr, check if they have permission to call the function in the module.


226    mapping(address => mapping(Role => bool)) public hasRole;  

229    mapping(address => mapping(Role => bool)) public hasRole;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L218



## [G-21] Using XOR (^) and AND (&) bitwise equivalents
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


## [G-22] Use assembly to perform efficient back-to-back calls
If a similar external call is performed back-to-back, we can use assembly to reuse any function signatures and function parameters that stay the same. In addition, we can also reuse the same memory space for each function call (scratch space + free memory pointer + zero slot), which can potentially allow us to avoid memory expansion costs.
Note: In order to do this optimization safely we will cache the free memory pointer value and restore it once we are done with our function calls. We will also set the zero slot back to 0 if neccessary.
[Reffrence](https://code4rena.com/reports/2023-05-ajna#g-10-use-assembly-to-perform-efficient-back-to-back-calls)

There are 1 instances of this issue:

```solidity
File:  src/policies/Factory.sol
124   ISafe(address(this)).enableModule(_stopPolicy);
127   ISafe(address(this)).setGuard(_guardPolicy);
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124-L127

## [G-23] Gas Optimization: Use `uint256(1)`/`uint256(2)` Instead for True and False Boolean States

**Explanation of Issue:**
When dealing with boolean states in Solidity, using `bool` variables can result in higher gas costs due to specific operations like Gwarmaccess (100 gas) and Gsset (20000 gas) when transitioning between 'false' and 'true', after having been 'true' in the past. To mitigate these gas costs, it is recommended to use `uint256(1)` and `uint256(2)` for representing 'true' and 'false', respectively.

**Examples of Code:**
  - *Non-optimized Code:*
    ```solidity
    bool isActive = true;
    ```
  - *Optimized Code:*
    ```solidity
    uint256 isActive = 1;
    ```

**Reason:**
The gas cost associated with changing a `bool` from 'false' to 'true' after it has been 'true' in the past involves both Gwarmaccess (100 gas) and Gsset (20000 gas). By using `uint256(1)` and `uint256(2)`, we avoid these gas-expensive operations, resulting in more efficient and cost-effective code. This optimization is particularly relevant in scenarios where frequent state changes occur.


There are 7 instances of this issue:
```solidity
File:  src/Create2Deployer.sol
50   deployed[targetDeploymentAddress] = true;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L50


```solidity
File:  src/Kernel.sol
319    if (!isRole[role_]) isRole[role_] = true;

322    hasRole[addr_][role_] = true;

342     hasRole[addr_][role_] = false;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L319


```solidity
File:  src/modules/PaymentEscrow.sol
62  initialized = true;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L62


```solidity
File:  src/modules/Storage.sol
90   initialized = true;

194  orders[orderHash] = true;
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L90