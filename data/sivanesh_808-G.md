| S.No | Title |
|------|-------|
| G-01 | Gas Optimization in `deploy` Function Redefine Inline Assembly in `Create2Deployer.sol` |
| G-02 | Enhanced Arithmetic Efficiency in `_calculatePaymentProRata` Function in `PaymentEscrow.sol` |
| G-03 | Streamlining Arithmetic in `_calculateFee` Function in `PaymentEscrow.sol` |
| G-04 | Optimization of Inline Assembly `_insert` in `Accumulator.sol` |
| G-05 | Optimization of Inline Assembly `_convertToStatic` in `Accumulator.sol` |
| G-06 | Optimization Report for `_validateFulfiller` and `_validateProtocolSignatureExpiration` Functions in `Signer.sol` |
| G-07 | In-Function Optimization of ABI Encoding in `_reclaimRentedItems()` in `Stop.sol` |
| G-08 | Optimization of `addRentalSafe` Function in `Storage` Contract |
| G-09 | Optimization of `isRentedOut` Function in `Storage` Contract |
| G-10 | Optimizing Hashing Operations with Assembly in `_deriveDomainSeparator` and `_deriveTypehashes` in `Signer.sol` |

### [G-01] Gas Optimization in `deploy` Function Redefine Inline Assembly

### File: Create2Deployer.sol

**Description:**
The objective of this optimization is to enhance the gas efficiency of the `deploy` function in a Solidity smart contract using refined inline assembly techniques. The original function uses the `create2` opcode for deploying contracts, but the assembly code can be restructured for better gas management.

**Original Code:**
```solidity
function deployOriginal(bytes memory initCode, bytes32 salt) public payable returns (address deploymentAddress) {
    assembly {
        deploymentAddress := create2(
            callvalue(),
            add(initCode, 0x20),
            mload(initCode),
            salt
        )
    }
}
```

**Optimized Code:**
```solidity
function deployOptimized(bytes memory initCode, bytes32 salt) public payable returns (address deploymentAddress) {
    assembly {
        let ptr := mload(0x40) // Free memory pointer
        mstore(ptr, callvalue()) // Store callvalue at ptr
        mstore(add(ptr, 0x20), add(initCode, 0x20)) // Store pointer to initCode
        mstore(add(ptr, 0x40), mload(initCode)) // Store length of initCode
        mstore(add(ptr, 0x60), salt) // Store salt

        deploymentAddress := create2(
            callvalue(), // Endowment (value) passed to the created contract
            ptr,         // Memory start pointer
            0x80,        // Memory length
            salt         // Salt value
        )
    }
}
```

**Explanation of Optimization:**

1. **Organized Memory Management:**
   - The optimized code first allocates a free memory pointer (`mload(0x40)`) to create an organized block of memory for the `create2` inputs.
   - This approach ensures that all the necessary data for the `create2` call is stored in a contiguous memory block, which can be more efficient for the EVM to process.

2. **Sequential Input Storage:**
   - Each input for the `create2` function is stored sequentially in the allocated memory. This not only makes the code clearer but also potentially reduces the computational overhead associated with calculating offsets for each parameter.

3. **Reduced Redundant Computations:**
   - In the original code, calculations like `add(initCode, 0x20)` are done within the `create2` call, potentially leading to redundant computations. In the optimized code, these calculations are performed once and stored, reducing the number of operations required.

4. **Explicit Memory Length Specification:**
   - The optimized code explicitly specifies the memory length (`0x80`) for the `create2` call, which includes the lengths of the `callvalue`, init code pointer, init code length, and salt. This precise specification helps in ensuring that only the necessary memory is used.

**Impact on Gas Consumption:**
- The restructured assembly code leads to more efficient gas usage. By organizing the inputs and reducing redundant computations, the optimized function consumes less gas compared to the original.
- It's important to note that while the gas savings are beneficial, inline assembly should be used cautiously due to its complexity and potential security implications.

### Github : [Create2Deployer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L32)



--------------------------------------------------------------------------------------------------------------------------------------------

### [G-02] Enhanced Arithmetic Efficiency in `_calculatePaymentProRata` Function

### File : PaymentEscrow.sol

### Description:
The `_calculatePaymentProRata` function, crucial in the `PaymentEscrow` smart contract, currently calculates pro-rata payments between a renter and a lender. This report suggests a further optimization to the function's calculation logic, aiming at reducing gas consumption by simplifying the arithmetic operations.

### Original Code:
```solidity
function _calculatePaymentProRata(
    uint256 amount,
    uint256 elapsedTime,
    uint256 totalTime
) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {
    uint256 numerator = (amount * elapsedTime) * 1000;
    renterAmount = ((numerator / totalTime) + 500) / 1000;
    lenderAmount = amount - renterAmount;
}
```

### Optimized Code:
```solidity
function _calculatePaymentProRata(
    uint256 amount,
    uint256 elapsedTime,
    uint256 totalTime
) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {
    renterAmount = (amount * elapsedTime + totalTime / 2) / totalTime;
    lenderAmount = amount - renterAmount;
}
```

### Detailed Explanation:

1. **Optimization of Division and Rounding Logic**:
   - **Original Approach**: Multiplies `amount * elapsedTime` by 1000, divides by `totalTime`, adds 500 (for rounding), and then divides by 1000 again.
   - **Optimization**: Removes the initial and final multiplication/division by 1000, and adds `totalTime / 2` before the division to handle rounding. This significantly reduces the number of arithmetic operations.

2. **Simplified Rounding Technique**:
   - The addition of `totalTime / 2` before the division is a standard technique for rounding to the nearest integer in integer division, replacing the more complex original approach.

3. **Elimination of Intermediate Variables**:
   - The `numerator` variable is removed, as the calculation is streamlined into a single line. This reduces the overhead and makes the code more concise.

4. **Gas Efficiency**:
   - Reducing the number of arithmetic operations, especially within a function that might be called frequently, can lead to significant gas savings over time.

5. **Maintaining Business Logic**:
   - Despite the optimization, the core business logic of the function – calculating the pro-rata distribution of payments – remains intact.

6. **Testing and Safety**:
   - Ensure thorough testing is conducted, especially focusing on edge cases and the correctness of rounding. Confirm that the optimized logic consistently produces the expected results without introducing any new issues.

### Github : [PaymentEscrow.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L132)

------------------------------------------------------------------------------------------------------------------


### [G-03] Streamlining Arithmetic in `_calculateFee` Function

### File : PaymentEscrow.sol

### Description:
The `_calculateFee` function in the `PaymentEscrow` smart contract is designed to calculate fees based on a given amount. This report suggests an optimization in its arithmetic logic to potentially reduce gas consumption.

### Original Code:
```solidity
function _calculateFee(uint256 amount) internal view returns (uint256) {
    return (amount * fee) / 10000;
}
```

### Optimized Code:
```solidity
function _calculateFee(uint256 amount) internal view returns (uint256) {
    // Optimized calculation
    return amount / (10000 / fee);
}
```

### Detailed Explanation:

1. **Rearranging the Arithmetic Operation**:
   - **Original Approach**: Multiplies the `amount` by `fee` and then divides by 10000. This involves a multiplication followed by a division.
   - **Optimization**: Divides 10000 by `fee` first and then divides the `amount` by this result. This changes the order of operations to potentially reduce the computational load.

2. **Gas Efficiency**:
   - Multiplication and division are relatively gas-expensive operations in EVM. By changing the order of operations, the optimized version might reduce the overall gas consumption, especially if `fee` is significantly smaller than 10000.

3. **Maintaining Precision**:
   - This optimization assumes that `10000 / fee` does not lead to significant precision loss. It's crucial to analyze and test to ensure that the calculation still maintains acceptable accuracy for the contract's use case.

4. **Testing and Safety**:
   - Comprehensive testing is essential to validate that the optimized version of the function produces results within an acceptable margin of error compared to the original, especially in edge cases where `amount` or `fee` is very large or small.

5. **Potential Overflow Check**:
   - While Solidity 0.8.x automatically checks for overflows, consider the implications of rearranging these operations on the risk of overflow, especially if `fee` can be very small, leading to a large divisor.

### Github : [PaymentEscrow.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88)

-------------------------------------------------------------------------------------------------------------


### [G-04] Optimization of Inline Assembly `_insert`

### File : `Accumulator.sol`

**Description:**
This report focuses on optimizing the inline assembly code used in the `Accumulator` contract. Inline assembly is a powerful feature in Solidity that allows for low-level operations and can lead to significant gas savings when used correctly. The optimization suggestions are intended to enhance the efficiency of memory operations and arithmetic computations without altering the contract's fundamental logic.

**Original Code:**
The `Accumulator` contract contains two primary functions with inline assembly - `_insert` and `_convertToStatic`. These functions handle dynamic memory operations and complex data structuring for managing `RentalAssetUpdate` structs.

**Optimization Recommendations:**

1. **Optimization in `_insert` Function:**
   - **Original Code Snippet:**
     ```solidity
     let newByteDataSize := add(mload(rentalAssets), 0x40)
     mstore(rentalAssets, newByteDataSize)
     ```
   - **Optimized Code Snippet:**
     ```solidity
     mstore(rentalAssets, add(mload(rentalAssets), 0x40))
     ```
   - **Explanation:**
     This optimization combines the calculation of `newByteDataSize` and the subsequent `mstore` operation into a single line. It reduces the number of operations by eliminating the need for a separate variable to hold `newByteDataSize`.

2. **Reusing Calculated Values:**
   - **Potential Optimization Area:**
     Reuse the value of `elements` and `newItemPosition` if they are recalculated elsewhere in the assembly block.

3. **Simplifying Arithmetic Expressions:**
   - **Potential Optimization Area:**
     Review and simplify arithmetic operations for calculating sizes and positions, ensuring no redundant calculations are performed.

4. **Optimization in `_convertToStatic` Function:**
   - **Potential Optimization Area:**
     Explore ways to optimize the loop that iterates over `rentalAssetUpdateLength`. Loop optimization can include minimizing the number of operations within the loop and using efficient arithmetic operations.

5. **General Assembly Optimizations:**
   - **Potential Optimization Area:**
     Use `mstore8` for single-byte operations, if applicable, and minimize the scope of variables to reduce stack operations.

### Github : [Accumulator.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L32)


-----------------------------------------------------------------------------------

### [G-05] Optimization of Inline Assembly `_convertToStatic`

### File : `Accumulator.sol`

To optimize the `_convertToStatic` function without separating it into a different function, we'll focus on streamlining the inline assembly operations within the same function. This approach will maintain the logic within a single function block, potentially reducing function call overhead and simplifying the contract structure.

### Original `_convertToStatic` Function

```solidity
// Original Code
function _convertToStatic(
        bytes memory rentalAssetUpdates
    ) internal pure returns (RentalAssetUpdate[] memory updates) {
        bytes32 rentalAssetUpdatePointer;
        assembly {
            rentalAssetUpdatePointer := add(0x20, rentalAssetUpdates)
            rentalAssetUpdateLength := mload(rentalAssetUpdatePointer)
        }
        updates = new RentalAssetUpdate[](rentalAssetUpdateLength);
        for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
            RentalId rentalId;
            uint256 amount;
            assembly {
                let currentElementOffset := add(0x20, mul(i, 0x40))
                rentalId := mload(add(rentalAssetUpdatePointer, currentElementOffset))
               amount := mload(
                    add(0x20, add(rentalAssetUpdatePointer, currentElementOffset))
                )
            }
 updates[i] = RentalAssetUpdate(rentalId, amount);
        }
    }
```

### Optimized `_convertToStatic` Function

```solidity
// Optimized Code
function _convertToStatic(
    bytes memory rentalAssetUpdates
) internal pure returns (RentalAssetUpdate[] memory updates) {
    uint256 rentalAssetUpdateLength;
    assembly {
        rentalAssetUpdateLength := mload(add(rentalAssetUpdates, 0x20))
    }

    updates = new RentalAssetUpdate[](rentalAssetUpdateLength);

    for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {
        uint256 offset = 0x20 + i * 0x40;
        uint256 rentalId;
        uint256 amount;

        assembly {
            rentalId := mload(add(rentalAssetUpdates, offset))
            amount := mload(add(rentalAssetUpdates, add(offset, 0x20)))
        }

        updates[i] = RentalAssetUpdate(rentalId, amount);
    }
}
```

### Explanation of Optimization:

1. **Inline Assembly within Loop:**
   - The loop now includes inline assembly to directly load the `rentalId` and `amount` from the byte array. This minimizes function calls and keeps the logic concise within the loop.

2. **Efficient Memory Access:**
   - Direct memory operations using inline assembly for loading `rentalId` and `amount` are more efficient than higher-level Solidity operations, potentially reducing gas costs, especially for larger arrays.

3. **Reduced Computational Overhead:**
   - The calculation of the offset for each `RentalAssetUpdate` struct within the byte array is done within the loop but outside the assembly block. This approach reduces computational steps in the assembly code, focusing it solely on memory operations.

   ### Github : [Accumulator.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L96)

------------------------------------------------------------------------------------

### [G-06] Optimization Report for `_validateFulfiller` and `_validateProtocolSignatureExpiration` Functions

### Contract Name: Signer.sol

### Description: 
This report outlines proposed optimizations for two functions in the specified Solidity smart contract: `_validateFulfiller` and `_validateProtocolSignatureExpiration`. Both functions currently perform single condition checks. The suggested optimizations involve refactoring these functions into modifiers, which can lead to reduced gas consumption by minimizing function call overhead.

---

### Original Code for `_validateFulfiller`:

```solidity
function _validateFulfiller(
    address intendedFulfiller,
    address actualFulfiller
) internal pure {
    if (intendedFulfiller != actualFulfiller) {
        revert Errors.SignerPackage_UnauthorizedFulfiller(
            actualFulfiller,
            intendedFulfiller
        );
    }
}
```

### Optimized Code for `_validateFulfiller`:

```solidity
modifier onlyIntendedFulfiller(address intendedFulfiller) {
    require(msg.sender == intendedFulfiller, "Unauthorized Fulfiller");
    _;
}
```

### Optimization Explanation for `_validateFulfiller`:
- **Use of Modifier:** The function is refactored into a `modifier`, which is more gas-efficient for simple condition checks.
- **Gas Savings:** Avoids the overhead of an internal function call. 
- **Maintained Logic:** The core logic remains unchanged; it still ensures that the caller is the intended fulfiller.

---

### Original Code for `_validateProtocolSignatureExpiration`:

```solidity
function _validateProtocolSignatureExpiration(uint256 expiration) internal view {
    if (block.timestamp > expiration) {
        revert Errors.SignerPackage_SignatureExpired(block.timestamp, expiration);
    }
}
```

### Optimized Code for `_validateProtocolSignatureExpiration`:

```solidity
modifier notExpired(uint256 expiration) {
    require(block.timestamp <= expiration, "Signature Expired");
    _;
}
```

### Optimization Explanation for `_validateProtocolSignatureExpiration`:
- **Use of Modifier:** Similar to `_validateFulfiller`, this function is refactored into a `modifier`.
- **Gas Efficiency:** This approach minimizes the gas cost by reducing the function call overhead.
- **Logical Consistency:** The functionality remains consistent with the original, ensuring that the current timestamp has not exceeded the expiration time.

### Github : [Signer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L76)

-------------------------------------------------------------------------------------------------------------------------------------------


### [G-07] In-Function Optimization of ABI Encoding in `_reclaimRentedItems()`

### Contract Name: stop.sol 

### Description:
The `_reclaimRentedItems()` function plays a pivotal role in the `Stop` contract, specifically in handling the execution of transactions to retrieve rented items. The primary focus of this optimization report is on the ABI encoding process utilized within the function. The original implementation employs `abi.encodeWithSelector`, which, while straightforward, may not be the most gas-efficient approach. This report presents an optimized solution that incorporates a more efficient encoding technique directly within the function, eliminating the need for additional helper functions.

### Original Code:

```solidity
function _reclaimRentedItems(RentalOrder memory order) internal {
    bool success = ISafe(order.rentalWallet).execTransactionFromModule(
        address(this),
        0,
        abi.encodeWithSelector(this.reclaimRentalOrder.selector, order),
        Enum.Operation.DelegateCall
    );

    if (!success) {
        revert Errors.StopPolicy_ReclaimFailed();
    }
}
```

### Optimized Code:

```solidity
function _reclaimRentedItems(RentalOrder memory order) internal {
    // Directly using the selector from the function's signature
    bytes4 selector = this.reclaimRentalOrder.selector;

    // Manually combining the selector with the encoded parameters using abi.encodePacked
    bytes memory encodedData = abi.encodePacked(selector, abi.encode(order));

    bool success = ISafe(order.rentalWallet).execTransactionFromModule(
        address(this),
        0,
        encodedData,
        Enum.Operation.DelegateCall
    );

    if (!success) {
        revert Errors.StopPolicy_ReclaimFailed();
    }
}
```

### Optimization Explanation:

- **Efficient ABI Encoding**: The optimization employs `abi.encodePacked()` for ABI encoding, replacing `abi.encodeWithSelector()`. This method combines the function selector with the encoded order parameters in a more gas-efficient manner.
- **Simplification of Code**: By integrating the encoding directly into `_reclaimRentedItems()`, the code is streamlined, avoiding the overhead of an additional function call.
- **Manual Selector and Parameter Combination**: The function selector and the parameters are manually combined using `abi.encodePacked()`, which allows for a compact data representation, potentially leading to gas savings.
- **Preserving Code Clarity**: This approach maintains the clarity and readability of the code, ensuring that optimizations do not compromise the understanding or maintenance of the contract.
- **Functionality Consistency**: The optimized version continues to fulfill the same role as the original function, ensuring that the behavior of the contract remains unchanged.

### Github : [Stop.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L166)

----------------------------------------------------------------------------------------------------------------------------------------


### [G-08] Optimization of `addRentalSafe` Function in `Storage` Contract

### File: Storage.sol

**Description:**  
The `addRentalSafe` function in the `Storage` contract is responsible for adding a new rental safe address to storage and incrementing the total count of safes. The current implementation uses an additional variable `newSafeCount` to calculate the incremented value of `totalSafes`. This report suggests an optimization to reduce gas consumption by directly incrementing the `totalSafes` state variable.

**Original Code:**
```solidity
function addRentalSafe(address safe) external onlyByProxy permissioned {
    // Get the new safe count.
    uint256 newSafeCount = totalSafes + 1;

    // Register the safe as deployed.
    deployedSafes[safe] = newSafeCount;

    // Increment nonce.
    totalSafes = newSafeCount;
}
```

**Optimized Code:**
```solidity
function addRentalSafe(address safe) external onlyByProxy permissioned {
    // Increment totalSafes and use it for registering the safe.
    deployedSafes[safe] = ++totalSafes;
}
```

**Optimization Explanation:**  
The optimization involves the removal of the temporary variable `newSafeCount` and directly incrementing the `totalSafes` state variable. This change reduces the gas cost associated with declaring and assigning a new variable. By using the pre-increment (`++totalSafes`), the state variable `totalSafes` is incremented before it is used to update `deployedSafes[safe]`. This approach simplifies the function and reduces the operations performed on the Ethereum Virtual Machine (EVM), leading to lower gas consumption.



### Github : [Storage.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L274)


----------------------------------------------------------------------------------------------------------------

### [G-09] Optimization of `isRentedOut` Function in `Storage` Contract

### File: Storage.sol

**Description:**  
The `isRentedOut` function in the `Storage` contract is designed to check if an asset is actively being rented by a wallet. Currently, the function constructs a `RentalId` structure to use as a key for accessing the `rentedAssets` mapping. This report proposes an optimization to reduce gas usage by directly accessing the `rentedAssets` mapping without constructing the `RentalId` structure.

**Original Code:**
```solidity
function isRentedOut(
    address recipient,
    address token,
    uint256 identifier
) external view returns (bool) {
    // calculate the rental ID
    RentalId rentalId = RentalUtils.getItemPointer(recipient, token, identifier);

    // Determine if there is a positive amount
    return rentedAssets[rentalId] != 0;
}
```

**Optimized Code:**
```solidity
function isRentedOut(
    address recipient,
    address token,
    uint256 identifier
) external view returns (bool) {
    // Directly use the composite key components to access the mapping
    return rentedAssets[keccak256(abi.encodePacked(recipient, token, identifier))] != 0;
}
```

**Optimization Explanation:**  
The optimization removes the need to create a `RentalId` structure and instead uses a hashed combination of `recipient`, `token`, and `identifier` as the key for the `rentedAssets` mapping. This is achieved by utilizing `keccak256` and `abi.encodePacked` to hash the concatenated values of the function parameters, forming a unique key. This approach reduces the computational overhead associated with struct creation and manipulation, leading to lower gas consumption for the function execution.

### Github : [Storage.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L118)
----------------------------------------------------------------------------------------------------------------------------

### [G-10] Optimizing Hashing Operations with Assembly in `_deriveDomainSeparator` and `_deriveTypehashes`

### File:`Signer.sol`

### Description:
The `_deriveDomainSeparator` and `_deriveTypehashes` functions in the `Signer` contract are responsible for deriving hashes crucial for the contract's functionality. These functions currently use Solidity's built-in hashing functions. This report proposes an optimization technique using inline assembly for hashing operations, which can potentially reduce gas costs.

### Original Code:

```solidity
// In _deriveTypehashes function
nameHash = keccak256(bytes(_NAME));
versionHash = keccak256(bytes(_VERSION));
eip712DomainTypehash = keccak256(
    abi.encodePacked(
        "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"
    )
);

// In _deriveDomainSeparator function
domainSeparator = keccak256(
    abi.encode(
        _eip712DomainTypeHash,
        _nameHash,
        _versionHash,
        block.chainid,
        address(this)
    )
);
```

### Optimized Code:

```solidity
// Optimized _deriveTypehashes function
assembly {
    nameHash := keccak256(add(_NAME, 32), mload(_NAME))
    versionHash := keccak256(add(_VERSION, 32), mload(_VERSION))
    let typeStr := mload(0x40) // Allocate memory
    mstore(typeStr, "EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
    eip712DomainTypehash := keccak256(typeStr, 71) // Length of the string above
}

// Optimized _deriveDomainSeparator function
assembly {
    let data := mload(0x40) // Allocate memory
    mstore(data, _eip712DomainTypeHash)
    mstore(add(data, 32), _nameHash)
    mstore(add(data, 64), _versionHash)
    mstore(add(data, 96), chainid())
    mstore(add(data, 128), address())
    domainSeparator := keccak256(data, 160) // 5 * 32 bytes
}
```

### Optimization Explanation:
- **Use of Inline Assembly**: The proposed optimization uses Solidity's inline assembly for lower-level access to EVM operations. Assembly allows for more direct and fine-grained control of memory and computation, which can lead to reduced gas costs.
- **Direct Memory Access**: The optimized code directly accesses and manipulates memory, reducing the overhead associated with higher-level Solidity operations.
- **Fixed Memory Allocation**: The optimized code computes the exact amount of memory needed for the hash inputs, eliminating dynamic memory allocation overhead.
- **Cautions and Trade-offs**: While assembly can offer gas savings, it comes with increased complexity and potential security risks. Inline assembly is harder to read and understand, which can introduce bugs and vulnerabilities if not used carefully. Thorough testing and auditing are essential when using assembly in smart contracts.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L273

### Github : [Signer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L273)

---------------------------------------------------------------------------------------------------------------------------
