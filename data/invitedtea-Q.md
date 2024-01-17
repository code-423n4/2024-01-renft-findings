# [L1] Potential reentrancy in PaymentEscrow settlePayment functions

The `settlePayment` and `settlePaymentBatch` functions in the `PaymentEscrow` contract call `_safeTransfer` which in turn makes external calls to the ERC20 token contracts to transfer funds. These external calls can potentially lead to reentrancy attacks if the ERC20 token contracts are malicious or contain unexpected behavior. There is no reentrancy guard present to protect against such attacks.

function settlePayment(RentalOrder calldata order) external onlyByProxy permissioned { ... _safeTransfer(token, lender, lenderAmount); ... _safeTransfer(token, renter, renterAmount); ... }
Lines 190-209 in PaymentEscrow contract (https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol)

function settlePaymentBatch(RentalOrder[] calldata orders) external onlyByProxy permissioned { ... _settlePayment( ... ); ... }
Lines 214-224 in PaymentEscrow contract (https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol)


---

# [L2] Policy Moderator Role Check

https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L45

modifier onlyRole(bytes32 role) { require(kernel.hasRole(role, msg.sender), "ERR_NOT_AUTHORIZED"); _; }

The Policy base contract is designed to allow only addresses with certain roles to input calls to its functions. However, the moderator role check is directly relying on the Kernel's storage, which might not take into account role revocation or temporal permission adjustment in a dynamic setting.

---
# [L3] Potentially Inefficient Memory Management

https://github.com/re-nft/smart-contracts/blob/main/src/packages/Accumulator.sol#L120

mstore(0x40, add(msize(), 96))

The Accumulator abstract contract manages dynamic allocation of `RentalAssetUpdate` structures in memory. However, each insertion into the `rentalAssets` array updates the free memory pointer, which could be unnecessary if no additional memory allocations are performed after the last insertion.

---

# [L4] Potentially Unexpected Behavior When Deal with Empty Bytecode

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L202

`require(data.length >= 4, ...);`

When handling the `data` of a transaction in the `checkTransaction` function, there is no check for the case where the `data` provided is empty except ensuring it has at least 4 bytes (to contain a function selector). However, when an empty bytecode is used in conjunction with a delegate call, it could potentially result in the entire contract being executed (which includes its fallback function). This behavior might be unexpected and may lead to unwanted consequences unless explicitly intended and handled.
---

# [L5] Missing check for zone reentrancy

https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L701

Line 701 in the provided Create contract code

`external override onlyRole(\"SEAPORT\") returns (bytes4 validOrderMagicValue) {`

The 'validateOrder' function lacks reentrancy protection for its zone interface, which could lead to unexpected behaviors if reentered. Even though in this case it may not be a vulnerability due to the role-based check right after, it's still considered a best practice to prevent reentrancy for sensitive functions.

---

# [L6] Unnecessary Usage of `payable` in Safe Setup Call

Line 177 and GitHub URL: https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L177

`payable(address(0))`

In the `deployRentalSafe` function, within the initializer payload for `Safe.setup`, the parameter for payment receiver is set as a zero address with `payable` cast. Since the payment token and payment amount are both zero, marking the payment receiver as `payable` is redundant and might be misleading.


---

# [L7] Redundant Check for ItemType

Lines 78, 82 in Reclaimer.sol

https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L78-L82

`if (item.itemType == ItemType.ERC721) ... else if (item.itemType == ItemType.ERC1155) ...`

In the `reclaimRentalOrder` function loop, each `item.itemType` is checked individually for ERC721 and ERC1155 types but there is no else or else if condition which could suggest forgetting ItemType handling logic or defaulting to a no operation. Adding an else condition could reveal developer intentions more clearly and prevent future code changes from introducing errors.

---

# [L8] Consider Adding Functionality for Support of Other Item Types

Line 71 in Reclaimer.sol
https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L71

`function reclaimRentalOrder(RentalOrder calldata rentalOrder) external { ... }`

The current contract only supports ERC721 and ERC1155 tokens. If the protocol is extended to support other types of tokens or items, the `reclaimRentalOrder` function will need to be updated accordingly.

---

# [L9] Inefficient Storage of Rental Asset Updates

stopRentBatch function in https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

`_insert(rentalAssetUpdates, orders[i].items[j].toRentalId(orders[i].rentalWallet), orders[i].items[j].amount);`

The rentalAssetUpdates accumulator is recalculated for each rental stop in stopRentBatch, which could lead to unnecessary computational overhead. It could be more efficient to calculate updates once and apply them to all stopped rentals.

---

# [L10] Potential Reentrancy in upgrade and freeze Functions

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L340

`_upgrade(newImplementation);`

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L350

`_freeze();`

Although currently controlled by permissions, the 'upgrade' and 'freeze' functions don't follow the Checks-Effects-Interactions pattern, potentially allowing reentrancy. While the current code's permission system should prevent unauthorized use, adherence to best practices is recommended.

---

# [L11] Unbounded iterations over dynamic arrays
## Impact
The impact of unbounded iterations over dynamic arrays in the mentioned functions (`_processBaseOrderOffer`, `_processPayOrderOffer`, `_processBaseOrderConsideration`, `_processPayeeOrderConsideration`, and `_addHooks`) is now considered low. While these functions iterate over arrays without an upper bound, which could potentially lead to out-of-gas errors for very large arrays, the likelihood and overall impact of such scenarios are limited. In most practical use cases, arrays passed to these functions are expected to be of reasonable size, thus reducing the risk of out-of-gas issues or denial of service attacks.

## Proof of Concept
The issue occurs in the following function calls:

1. `_processBaseOrderOffer(items, offers, 0);`
   [Line 263 and Line 168 in Create Contract Code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L263)

2. `_processPayOrderOffer(items, offers, 0);`
   [Line 285 and Line 182 in Create Contract Code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L285)

3. `_processBaseOrderConsideration(items, considerations, offers.length);`
   [Line 356 and Line 204 in Create Contract Code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L356)

4. `_processPayeeOrderConsideration(considerations);`
   [Line 381 and Line 224 in Create Contract Code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7

"The `_processBaseOrderOffer`, `_processPayOrderOffer`, `_processBaseOrderConsideration`, `_processPayeeOrderConsideration`, and `_addHooks` functions iterate over arrays without ensuring that they have an upper bound. This could lead to out-of-gas errors for very large arrays or be susceptible to denial of service attacks.

Given the low impact of this issue, the mitigation steps can be less rigorous compared to higher severity issues. However, it is still good practice to consider adding checks or limitations on the size of the arrays being processed to avoid potential denial of service:

Implement a check on the length of the arrays being passed to these functions. This can be a maximum length constant or a dynamic limit based on gas usage.

Example:
```solidity
require(offers.length <= MAX_OFFERS_LENGTH, "Too many offers");
```
---

# [L-12] Potential for Invalid Safe Threshold

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L160

`if (threshold == 0 || threshold > owners.length) {...`

In the `deployRentalSafe` function, the threshold for transaction approvals is checked to ensure it is greater than 0 and not more than the number of `owners`. However, there is no validation to confirm the uniqueness of each `owner` in the provided array. The absence of such a check could lead to a scenario where duplicate entries in the `owners` array potentially reduce the effective security of the safe. This situation could result in fewer unique signatures being required to meet the threshold than originally intended, compromising the intended security level. The impact of this issue is downgraded to low severity because it requires a specific and less likely set of circumstances to pose a real threat — namely, the unintentional or unnoticed inclusion of duplicate owner addresses.

## Recommended Mitigation Steps

To address this potential issue, it is advised to include a check for the uniqueness of the `owners` array entries. This can be achieved by implementing a simple mapping to track the addresses already encountered in the array. If a duplicate address is detected, the function should revert to prevent the establishment of a safe with


---

# [G-01] Gas: Inefficient Storage Read in `_addHooks`

The function `_addHooks` in the `Create` contract accesses `STORE.hookOnStart(target)` within a loop for every hook, which can lead to repeated and unnecessary storage reads. This inefficiency can be optimized by reading the value once before the loop, especially when dealing with duplicate entries in hooks. This optimization can lead to potentially significant gas savings if there are many hooks with repeated values.

The issue is located at:
`if (!STORE.hookOnStart(target)) {`
[Line 480 in Create Contract](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L480)

## Recommended Mitigation Steps

To optimize gas usage, consider refactoring the `_addHooks` function to read the `hookOnStart` status of each target only once prior to the loop. This can be achieved by storing the statuses in a local mapping or array before entering the loop. The modified function would first check the mapping or array instead of directly querying the storage each time within the loop.


# [G-02] Gas Optimization Suggestion for `configureDependencies`

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L81

In the `configureDependencies` function of a contract, there is a potential area for gas optimization. The function currently allocates a new array for dependencies and calls `toKeycode` for "STORE" on each instance. This approach incurs additional gas costs with each call. To optimize for gas efficiency, it is suggested to store the keycode as a constant, and the array allocation can be minimized by initializing it directly with the required values.

The issue is present in these lines:
```solidity
dependencies = new Keycode[](1);
dependencies[0] = toKeycode("STORE");
```
To optimize gas usage, the following changes are recommended:

Store Keycode as Constant: Instead of calling toKeycode("STORE") each time, define it as a constant at the contract level. This reduces the computational overhead and gas costs associated with dynamically computing the keycode.

Optimize Array Initialization: Directly initialize the dependencies array with the required values. For instance:
```solidity
// Define the STORE keycode as a constant
Keycode constant STORE_KEYCODE = toKeycode("STORE");

function configureDependencies() external override returns (Keycode[] memory dependencies) {
    dependencies = new Keycode[](1) { STORE_KEYCODE };
}

```
---

# [G-03] Inefficient Storage Access Pattern in `removeRentalsBatch`

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L251

The `removeRentalsBatch` function in the specified contract exhibits an inefficient pattern of storage access. Within a loop, the function repeatedly checks the same storage location (`orders[orderHashes[i]]`) for each iteration. This repetitive querying of storage can result in unnecessary gas consumption. A more efficient approach would be to cache the value of `orders[orderHashes[i]]` in a local variable outside of the loop, thereby reducing the number of storage accesses.

The issue is identified in the following lines:
```solidity
if (!orders[orderHashes[i]]) {
    ...
} else {
    delete orders[orderHashes[i]];
}
```



Cache Storage Value: Before entering the loop, declare a local variable to cache the value of orders[orderHashes[i]]. This prevents repeated and unnecessary storage reads.

Use Cached Value in Loop: Utilize the cached value within the loop for checks and operations, rather than directly accessing the storage each time.

```solidity
function removeRentalsBatch(bytes32[] calldata orderHashes) external {
    for (uint256 i = 0; i < orderHashes.length; ++i) {
        bool isOrderActive = orders[orderHashes[i]];
        if (!isOrderActive) {
            ...
        } else {
            delete orders[orderHashes[i]];
        }
    }
}
```

---

# [G-04] Unnecessary Reassignment of Memory in Loop

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L313

In the `stopRentBatch` function, found in the smart contract, there is an observed inefficiency where `rentalAssetUpdates` is reassigned in every iteration of the loop. This pattern leads to unnecessary memory reassignment and thereby consumes more gas than required. Optimizing this process by computing the update once outside the loop could result in reduced gas usage.

The issue can be found in the following section of the code:
```solidity
bytes memory rentalAssetUpdates = new bytes(0);
```

Compute Update Outside Loop: Modify the stopRentBatch function to compute rentalAssetUpdates once outside the loop. This approach avoids the need to reassign memory in each iteration, thereby saving gas.

Refactor Function Logic: Adjust the logic of the function to accumulate the necessary updates before entering the loop. This could involve aggregating the required data in a temporary structure and then processing it all at once after the loop.

```solidity
function stopRentBatch(...) {
    // Pre-compute rentalAssetUpdates
    bytes memory rentalAssetUpdates = ...; // Pre-loop computation

    for (uint256 i = 0; i < orders.length; ++i) {
        // Loop logic using the pre-computed rentalAssetUpdates
        ...
    }
    // Post-loop processing if necessary
}

```


# [R-01] Refactoring: Unnecessary Logical Branching in '_convertToItems'

In the `_convertToItems` function of the `Create` contract, the use of 'else if' for checking `orderType.isPayOrder()` can be simplified. Since there is a preceding return statement, this 'else if' can be replaced with a standard 'if' statement, improving the readability of the code. 

The issue is located at:
`else if (orderType.isPayOrder()) {`
[Line 428 in Create Contract](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L428)

## Recommended Mitigation Steps

Refactor the `_convertToItems` function by replacing the 'else if' statement with an 'if' statement. This change will make the code more readable and straightforward, as each condition is independently evaluated, and the function's flow becomes clearer. This refactoring does not impact the functionality but enhances the maintainability and clarity of the code.

---

# [R-02] Inconsistent Use of `revert` Statements in Create2Deployer Contract

Throughout the `Create2Deployer` contract, there is an inconsistency in the use of `revert` statements. The pattern observed involves `revert` statements being directly followed by calls to an `Errors` library, which instantiate custom error objects with specific error messages. This approach differs from typical Solidity development practices, where the use of custom error types is generally preferred. Utilizing custom error types is advantageous due to lower gas costs and improved code readability.

Instances of this issue include:
1. `revert Errors.Create2Deployer_UnauthorizedSender(msg.sender, salt);`
   [Line 38 in Create2Deployer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L24)
2. `revert Errors.Create2Deployer_AlreadyDeployed(targetDeploymentAddress, salt);`
   [Line 46 in Create2Deployer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L34)
3. `revert Errors.Create2Deployer_MismatchedDeploymentAddress(targetDeploymentAddress, deploymentAddress);`
   [Line 68 in Create2Deployer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L57)

## Recommended Mitigation Steps

The contract should be refactored to use custom error types consistently. This involves defining error types in the `Errors` library and then using them directly in `revert` statements. This refactoring will not only align the code with standard Solidity practices but also optimize for gas efficiency and enhance code legibility.

For example, the refactoring can be done as follows:

```solidity
// In Errors.sol
error UnauthorizedSender(address sender, bytes32 salt);
error AlreadyDeployed(address targetDeploymentAddress, bytes32 salt);
error MismatchedDeploymentAddress(address targetDeploymentAddress, address deploymentAddress);

// Usage in Create2Deployer.sol
if (condition) {
    revert UnauthorizedSender(msg.sender, salt);
}
```
---

# [R-03] Use of External for Functions That Should Be Public

In the smart contract, certain functions like `addRentals` and `removeRentals` are marked as `external` but are expected to be called internally by the proxy. The current visibility of these functions as `external` could lead to potential confusion and misunderstanding about their intended use. It is generally more appropriate to mark functions that are intended to be accessible both internally and externally as `public`. This change would clarify the functions' intended use and ensure that they are accessible in the expected contexts.

Instances of this issue include:
1. `function addRentals(...) external onlyByProxy permissioned {...}`
   [Lines 189 in Storage.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L189)
2. `function removeRentals(...) external onlyByProxy permissioned {...}`
   [Lines 216 in Storage.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L216)

## Recommended Mitigation Steps

To address this issue, the following refactoring is recommended:

1. **Change Visibility to Public**: Update the visibility modifier of the `addRentals` and `removeRentals` functions from `external` to `public`. This change makes it explicit that these functions are intended to be accessible both from external calls and from within the contract or its derivatives.

For example, the refactored functions might look like this:

```solidity
function addRentals(...) public onlyByProxy permissioned {...}

function removeRentals(...) public onlyByProxy permissioned {...}
···
---


