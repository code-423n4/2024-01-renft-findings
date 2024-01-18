##  Gas Summary

| Number | Issue | Instances|
|--------|-------|----------|
|[G-01]|  Using storage instead of memory for structs/arrays saves gas   | 11 |
|[G-02]|  Use assembly to reuse memory space when making more than one external call   | 1 |
|[G-03]|  It is sometimes cheaper to cache calldata   | 12 |
|[G-04]|  Save loop calls   | 2 |
|[G-05]|  Use the existing Local variable/global variable when equal to a state variable to avoid reading from state   | 1 |
|[G-06]|  Use uint256(1)/uint256(2) instead for true and false boolean sta   | 10 |
|[G-07]|  Efficient data management:   | 1 |
|[G-08]|  Don’t make variables public unless necessary   | 11 |
|[G-09]|  Avoid Unnecessary Use of Storage  | 2 |
|[G-10]|  Create immutable variable to avoid an external call   | 8 |
|[G-11]|  Use assembly to reuse memory space when making more than one external call   | 3 |
|[G-12]|  Use calldata instead of memory for function parameters   | 14 |
|[G-13]|  Use assembly for loops   | 13 |
|[G-14]|  Cache function calls   | 2 |
|[G-15]|  Use hardcode address instead address(this)   | 10 |
|[G-16]|  Using assembly to revert with an error message   | 49 |




## [G-01] Using storage instead of memory for structs/arrays saves gas 

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol
 
424   Permissions[] memory requests = policy_.requestPermissions();

434   Keycode[] memory dependencies = policy_.configureDependencies();

462   Permissions[] memory requests = policy_.requestPermissions();

542   Policy[] memory dependents = moduleDependents[keycode_];

568   Permissions memory request = requests_[i];

588   Keycode[] memory dependencies = policy_.configureDependencies();

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L424

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol

166   bytes32[] memory itemHashes = new bytes32[](order.items.length);

167    bytes32[] memory hookHashes = new bytes32[](order.hooks.length);

222    bytes32[] memory hookHashes = new bytes32[](metadata.hooks.length);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L166

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

315   bytes32[] memory orderHashes = new bytes32[](orders.length)

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L315

## [G-02] Use assembly to reuse memory space when making more than one external call

Issue Description - When making external calls, the solidity compiler has to encode the function signature and arguments in memory. It does not clear or reuse memory, so it expands memory each time.

Proposed Optimization - Use inline assembly to reuse the same memory space for multiple external calls. Store the function selector and arguments without expanding memory further.

Estimated Gas Savings - Reusing memory can save thousands of gas compared to expanding on each call. The baseline memory expansion per call is 3,000 gas. With larger arguments or return data, the savings would be even greater.


```solidity

See the example below;
contract Called {
    function add(uint256 a, uint256 b) external pure returns(uint256) {
        return a + b;
    }
}
contract Solidity {
    // cost: 7262
    function call(address calledAddress) external pure returns(uint256) {
        Called called = Called(calledAddress);
        uint256 res1 = called.add(1, 2);
        uint256 res2 = called.add(3, 4);
        uint256 res = res1 + res2;
        return res;
    }
}
contract Assembly {
    // cost: 5281
    function call(address calledAddress) external view returns(uint256) {
        assembly {
            // check that calledAddress has code deployed to it
            if iszero(extcodesize(calledAddress)) {
                revert(0x00, 0x00)
            }
            // first call
            mstore(0x00, hex"771602f7")
            mstore(0x04, 0x01)
            mstore(0x24, 0x02)
            let success := staticcall(gas(), calledAddress, 0x00, 0x44, 0x60, 0x20)
            if iszero(success) {
                revert(0x00, 0x00)
            }
            let res1 := mload(0x60)
            // second call
            mstore(0x04, 0x03)
            mstore(0x24, 0x4)
            success := staticcall(gas(), calledAddress, 0x00, 0x44, 0x60, 0x20)
            if iszero(success) {
                revert(0x00, 0x00)
            }
            let res2 := mload(0x60)
            // add results
            let res := add(res1, res2)
            // return data
            mstore(0x60, res)
            return(0x60, 0x20)
        }
    }
}
```

We save approximately 2,000 gas by using the scratch space to store the function selector and its arguments and also reusing the same memory space for the second call while storing the returned data in the zero slot thus not expanding memory.

If the arguments of the external function you wish to call is above 64 bytes and if you are making one external call, it wouldn’t save any significant gas writing it in assembly. However, if making more than one call. You can still save gas by reusing the same memory slot for the 2 calls using inline assembly.

Note: Always remember to update the free memory pointer if the offset it points to is already used, to avoid solidity overriding the data stored there or using the value stored there in an unexpected way.

Also note to avoid overwriting the zero slot (0x60 memory offset) if you have undefined dynamic memory values within that call stack. An alternative is to explicitly define dynamic memory values or if used, to set the slot back to 0x00 before exiting the assembly block.

Code Snippets:


```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

512          for (uint256 i; i < keycodeLen; ++i) {
            // get the module represented by the keycode.
            Module module = Module(getModuleForKeycode[allKeycodes[i]]);
            // Instruct the module to change the kernel.
            module.changeKernel(newKernel_);
        }

        // For each active policy stored in the kernel
        uint256 policiesLen = activePolicies.length;
        for (uint256 j; j < policiesLen; ++j) {
            // Get the policy.
            Policy policy = activePolicies[j];

            // Deactivate the policy before changing kernel.
            policy.setActiveStatus(false);

            // Instruct the policy to change the kernel.
            policy.changeKernel(newKernel_);
        }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L512-L530

## [G-03] It is sometimes cheaper to cache calldata

Issue Description - While the calldataload opcode is relatively cheap, directly using it in a loop or multiple times can still result in unnecessary bytecode. Caching the loaded calldata first may allow for optimization opportunities.

Proposed Optimization - Cache calldata values in a local variable after first load, then reference the local variable instead of repeatedly using calldataload.

Estimated Gas Savings - Exact savings vary depending on contract, but caching calldata parameters can save 5-20 gas per usage by avoiding extra calldataload opcodes. Larger functions with many parameter uses see more benefit.

Code Snippet:

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol

218   RentalAssetUpdate[] calldata rentalAssetUpdates

245   bytes32[] calldata orderHashes,

246   RentalAssetUpdate[] calldata rentalAssetUpdates

```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L218


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

195  Hook[] calldata hooks,

196  Item[] calldata rentalItems,

265  function stopRent(RentalOrder calldata order) external {

313  function stopRentBatch(RentalOrder[] calldata orders) external {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L195

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol

139   address[] calldata owners,

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L139

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

734   ZoneParameters calldata zoneParams

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L734

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

216   Item[] calldata items,

320   function settlePayment(RentalOrder calldata order) external onlyByProxy permissioned {

338   RentalOrder[] calldata orders

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L216

## [G-04] Save loop calls

Instead of calling  moduleDependents[keycode] 2 times in each loop for fetching data, and three time in _pruneFromDependents() function  it can be saved as a variable.

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

438   for (uint256 i; i < depLength; ++i) {
            Keycode keycode = dependencies[i];

            // Push the policy to the array of dependents for the keycode
            moduleDependents[keycode].push(policy_);

            // Set the index of the policy in the array of dependents.
            getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1;
        }

592  for (uint256 i; i < depcLength; ++i) {
            // Get the stored array of policies that depend on the keycode.
            Keycode keycode = dependencies[i];
            Policy[] storage dependents = moduleDependents[keycode];

            // Get the index of the policy to prune in the array.
            uint256 origIndex = getDependentIndex[keycode][policy_];

            // Get the address of the last policy in the array.
            Policy lastPolicy = dependents[dependents.length - 1];

            // Overwrite the last policy with the policy being pruned.
            dependents[origIndex] = lastPolicy;

            // Since the last policy exists twice now in the array, pop it
            // from the end of the array.
            dependents.pop();

            // Set the index of the swapped policy to its correct spot.
            getDependentIndex[keycode][lastPolicy] = origIndex;

            // Delete the index of the of the pruned policy.
            delete getDependentIndex[keycode][policy_];
        }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L438-L446

## [G-05] Use the existing Local variable/global variable when equal to a state variable to avoid reading from state

### Local variable oldModule should be used instead of reading  getModuleForKeycode[keycode]

```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

387          // Get the address of the old module
        Module oldModule = getModuleForKeycode[keycode];

        // Check that the old module contract exists, and that the old module
        // address is not the same as the new module
        if (address(oldModule) == address(0) || oldModule == newModule_) {
            revert Errors.Kernel_InvalidModuleUpgrade(keycode);
        }

        // The old module no longer points to the keycode.
        getKeycodeForModule[oldModule] = Keycode.wrap(bytes5(0));

        // The new module points to the keycode.
        getKeycodeForModule[newModule_] = keycode;

        // The keycode points to the new module.
        getModuleForKeycode[keycode] = newModule_;

        // Initialize the new module contract.
        newModule_.INIT();

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L387-L486

## [G-06] Use uint256(1)/uint256(2) instead for true and false boolean states

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. see source:

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol

20  mapping(bytes32 orderHash => bool isActive) public orders;

55  mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;

58  mapping(address extension => bool isWhitelisted) public whitelistedExtensions;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L20

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

116   bool public isActive;

563    bool grant_

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L116

```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

228    bool isRentalOver = elapsedTime >= totalTime;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L228

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

132  bool hasExpired = endTimestamp <= block.timestamp;

135  bool isLender = expectedLender == msg.sender;

168  bool success = ISafe(order.rentalWallet).execTransactionFromModule(

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L132

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol

335  bool isActive = STORE.hookOnTransaction(hook);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L335

## [G-07] Efficient data management:

Permanently save a storage variable in memory in a function. Strategically use memory within functions for temporary storage of variables.

Store the dependents state variabl in memory variable

```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

595              Policy[] storage dependents = moduleDependents[keycode];

            // Get the index of the policy to prune in the array.
            uint256 origIndex = getDependentIndex[keycode][policy_];

            // Get the address of the last policy in the array.
            Policy lastPolicy = dependents[dependents.length - 1];

            // Overwrite the last policy with the policy being pruned.
            dependents[origIndex] = lastPolicy;

            // Since the last policy exists twice now in the array, pop it
            // from the end of the array.
            dependents.pop();

            // Set the index of the swapped policy to its correct spot.
            getDependentIndex[keycode][lastPolicy] = origIndex;

            // Delete the index of the of the pruned policy.
            delete getDependentIndex[keycode][policy_];
        }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L595-L615

## [G-08] Don’t make variables public unless necessary

Issue Description - Making variables public comes with some overhead costs that can be avoided in many cases. A public variable implicitly creates a public getter function of the same name, increasing the contract size.

Proposed Optimization - Only mark variables as public if their values truly need to be readable by external contracts/users. Otherwise, use private or internal visibility.

Estimated Gas Savings - The savings from avoiding unnecessary public variables are small per transaction, around 5-10 gas. However, this adds up over many transactions targeting a contract with public state variables that don’t need to be public.

Code Snippets:

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol

21  Storage public STORE;

22  PaymentEscrow public ESCRW;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L21

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

53   Storage public STORE;

54   PaymentEscrow public ESCRW;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L53

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol

28  Storage public STORE;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L28


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol

45   Storage public STORE;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L45

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

28   uint256 public fee;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L28

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol
 
25    Kernel public kernel;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L25

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

44  Storage public STORE;

45   PaymentEscrow public ESCRW;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L44

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol
 
36   uint256 public totalSafes;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L36

## [G-09] Avoid Unnecessary Use of Storage

Writing data to storage is one of the most expensive operations in Solidity. Reduce gas costs by avoiding unnecessary writes. For example, move constant values to memory:

```solidity

// Expensive
string public constant NAME = "MyContract"; 

// Cheaper
string memory NAME = "MyContract";

```

Also be careful about storing large pieces of data on-chain. Use alternative patterns like IPFS for larger data.

Storing data costs 20,000 gas versus 3 gas for memory.
Minimize storage by using memory for fixed constants and temporary values.
Store large data like files off-chain (IPFS) and put hash on-chain.

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol

25   string internal constant _NAME = "ReNFT-Rentals";

26   string internal constant _VERSION = "1.0.0";

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L25


## [G-10] Create immutable variable to avoid an external call

Instead of performing an external call to get the root address each time _enableNode is invoked, we can perform this external call once in the constructor and store the root as an immutable variable. Doing this will save 1 external call each time _enableNode is invoked.


Code Snippets:

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol

21  Storage public STORE;

22  PaymentEscrow public ESCRW;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L21

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

53   Storage public STORE;

54   PaymentEscrow public ESCRW;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L53

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol

28  Storage public STORE;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L28


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol

45   Storage public STORE;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L45

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol
 
25    Kernel public kernel;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L25

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

44  Storage public STORE;

45   PaymentEscrow public ESCRW;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L44

## [G-11] Use assembly to reuse memory space when making more than one external call

Issue Description - When making external calls, the solidity compiler has to encode the function signature and arguments in memory. It does not clear or reuse memory, so it expands memory each time.

Proposed Optimization - Use inline assembly to reuse the same memory space for multiple external calls. Store the function selector and arguments without expanding memory further.

Estimated Gas Savings - Reusing memory can save thousands of gas compared to expanding on each call. The baseline memory expansion per call is 3,000 gas. With larger arguments or return data, the savings would be even greater.


```solidity
See the example below;
contract Called {
    function add(uint256 a, uint256 b) external pure returns(uint256) {
        return a + b;
    }
}
contract Solidity {
    // cost: 7262
    function call(address calledAddress) external pure returns(uint256) {
        Called called = Called(calledAddress);
        uint256 res1 = called.add(1, 2);
        uint256 res2 = called.add(3, 4);
        uint256 res = res1 + res2;
        return res;
    }
}
contract Assembly {
    // cost: 5281
    function call(address calledAddress) external view returns(uint256) {
        assembly {
            // check that calledAddress has code deployed to it
            if iszero(extcodesize(calledAddress)) {
                revert(0x00, 0x00)
            }
            // first call
            mstore(0x00, hex"771602f7")
            mstore(0x04, 0x01)
            mstore(0x24, 0x02)
            let success := staticcall(gas(), calledAddress, 0x00, 0x44, 0x60, 0x20)
            if iszero(success) {
                revert(0x00, 0x00)
            }
            let res1 := mload(0x60)
            // second call
            mstore(0x04, 0x03)
            mstore(0x24, 0x4)
            success := staticcall(gas(), calledAddress, 0x00, 0x44, 0x60, 0x20)
            if iszero(success) {
                revert(0x00, 0x00)
            }
            let res2 := mload(0x60)
            // add results
            let res := add(res1, res2)
            // return data
            mstore(0x60, res)
            return(0x60, 0x20)
        }
    }
}
```

We save approximately 2,000 gas by using the scratch space to store the function selector and its arguments and also reusing the same memory space for the second call while storing the returned data in the zero slot thus not expanding memory.

If the arguments of the external function you wish to call is above 64 bytes and if you are making one external call, it wouldn’t save any significant gas writing it in assembly. However, if making more than one call. You can still save gas by reusing the same memory slot for the 2 calls using inline assembly.

Note: Always remember to update the free memory pointer if the offset it points to is already used, to avoid solidity overriding the data stored there or using the value stored there in an unexpected way.

Also note to avoid overwriting the zero slot (0x60 memory offset) if you have undefined dynamic memory values within that call stack. An alternative is to explicitly define dynamic memory values or if used, to set the slot back to 0x00 before exiting the assembly block.

Code Snippets:

use assembly for ReceivedItem memory execution = executions[i]; external call 

```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

695   for (uint256 i = 0; i < executions.length; ++i) {
            ReceivedItem memory execution = executions[i];

            // ERC20 invariant where the recipient must be the payment escrow.
            if (execution.isERC20()) {
                _checkExpectedRecipient(execution, address(ESCRW));
            }
            // ERC721 and ERC1155 invariants where the recipient must
            // be the expected rental safe.
            else if (execution.isRental()) {
                _checkExpectedRecipient(execution, expectedRentalSafe);
            }
            // Revert if unsupported item type.
            else {
                revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                    execution.itemType
                );
            }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L695-L712


use assembly for Keycode keycode = newModule_.KEYCODE(); external call

```solidity
file:   blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

356      function _installModule(Module newModule_) internal {
        // Fetch the module keycode.
        Keycode keycode = newModule_.KEYCODE();

        // Make sure the keycode isnt in use already.
        if (address(getModuleForKeycode[keycode]) != address(0)) {
            revert Errors.Kernel_ModuleAlreadyInstalled(keycode);
        }

        // Connect the keycode to the module address.
        getModuleForKeycode[keycode] = newModule_;

        // Connect the module address to the keycode.
        getKeycodeForModule[newModule_] = keycode;

        // Keep a running array of all module keycodes.
        allKeycodes.push(keycode);

        // Initialize the module contract.
        newModule_.INIT();
    }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L356-L376

use assembly for  Keycode keycode = newModule_.KEYCODE(); external call 

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

385          Keycode keycode = newModule_.KEYCODE();

        // Get the address of the old module
        Module oldModule = getModuleForKeycode[keycode];

        // Check that the old module contract exists, and that the old module
        // address is not the same as the new module
        if (address(oldModule) == address(0) || oldModule == newModule_) {
            revert Errors.Kernel_InvalidModuleUpgrade(keycode);
        }

        // The old module no longer points to the keycode.
        getKeycodeForModule[oldModule] = Keycode.wrap(bytes5(0));'
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L385-L397


## [G-12] Use calldata instead of memory for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory.

Note that I’ve also flagged instances where the function is public but can be marked as external since it’s not called by the contract, and cases where a constructor is involved.

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol
 
562   Permissions[] memory requests_,

```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L562


```solidity 
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

196  Item[] memory rentalItems,

197  SpentItem[] memory offers,

248  Item[] memory rentalItems,

249  SpentItem[] memory offers,

327  Item[] memory rentalItems,

328  ReceivedItem[] memory considerations,

368   ReceivedItem[] memory considerations

412  SpentItem[] memory offers,

413  ReceivedItem[] memory considerations,

465  Hook[] memory hooks,

466  SpentItem[] memory offerItems,

692   ReceivedItem[] memory executions,


```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L196

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol

191  RentalAssetUpdate[] memory rentalAssetUpdates

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L191

## [G-13] Use assembly for loops

In the following instances, assembly is used for more gas efficient loops. The only memory slots that are manually used in the loops are scratch space (0x00-0x20), the free memory pointer (0x40), and the zero slot (0x60). This allows us to avoid using the free memory pointer to allocate new memory, which may result in memory expansion costs.

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol

170   for (uint256 i = 0; i < order.items.length; ++i) {
            // Hash the item.
            itemHashes[i] = _deriveItemHash(order.items[i]);
        }

176    for (uint256 i = 0; i < order.hooks.length; ++i) {
            // Hash the hook.
            hookHashes[i] = _deriveHookHash(order.hooks[i]);
        }

225   for (uint256 i = 0; i < metadata.hooks.length; ++i) {
            // Hash the hook
            hookHashes[i] = _deriveHookHash(metadata.hooks[i]);
        }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L170-L173


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

599    for (uint256 i = 0; i < items.length; ++i) {
                if (items[i].isERC20()) {
                    ESCRW.increaseDeposit(items[i].token, items[i].amount);
                }
            }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L599-L603

```solidity
file:

438   for (uint256 i; i < depLength; ++i) {
            Keycode keycode = dependencies[i];

            // Push the policy to the array of dependents for the keycode
            moduleDependents[keycode].push(policy_);

            // Set the index of the policy in the array of dependents.
            getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1;
        }

512  for (uint256 i; i < keycodeLen; ++i) {
            // get the module represented by the keycode.
            Module module = Module(getModuleForKeycode[allKeycodes[i]]);
            // Instruct the module to change the kernel.
            module.changeKernel(newKernel_);
        }

521   for (uint256 j; j < policiesLen; ++j) {
            // Get the policy.
            Policy policy = activePolicies[j];

            // Deactivate the policy before changing kernel.
            policy.setActiveStatus(false);

            // Instruct the policy to change the kernel.
            policy.changeKernel(newKernel_);
        }
    
546   for (uint256 i; i < depLength; ++i) {
            // Reconfigure its dependencies.
            dependents[i].configureDependencies();
        }
        '
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L438-L446

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

341  for (uint256 i = 0; i < orders.length; ++i) {
            // Settle all payments for the order.
            _settlePayment(
                orders[i].items,
                orders[i].orderType,
                orders[i].lender,
                orders[i].renter,
                orders[i].startTimestamp,
                orders[i].endTimestamp
            );
        }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L341-L351


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol

197   for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Update the order hash for that item.
            rentedAssets[asset.rentalId] += asset.amount;
        }

229   for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }

449   for (uint256 i = 0; i < orderHashes.length; ++i) {
            // The order must exist to be deleted.
            if (!orders[orderHashes[i]]) {
                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
            } else {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            }
        }

260   for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }
        
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L197-L202

## [G-14] Cache function calls

we are calling changeKernel(newKernel_) function two times. cache the function to save gas 

```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

508     function _migrateKernel(Kernel newKernel_) internal {
        uint256 keycodeLen = allKeycodes.length;

        // For each keycode stored in the kernel.
        for (uint256 i; i < keycodeLen; ++i) {
            // get the module represented by the keycode.
            Module module = Module(getModuleForKeycode[allKeycodes[i]]);
            // Instruct the module to change the kernel.
            module.changeKernel(newKernel_);
        }

        // For each active policy stored in the kernel
        uint256 policiesLen = activePolicies.length;
        for (uint256 j; j < policiesLen; ++j) {
            // Get the policy.
            Policy policy = activePolicies[j];

            // Deactivate the policy before changing kernel.
            policy.setActiveStatus(false);

            // Instruct the policy to change the kernel.
            policy.changeKernel(newKernel_);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L508-L529 

we are calling _revertSelectorOnActiveRental(selector, from, to, tokenId); function 5 times and the function _revertNonWhitelistedExtension(extension) two time. cache the function to save gas 


```solidity
file:   blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol

205              uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e721_safe_transfer_from_1_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);
        } else if (selector == e721_safe_transfer_from_2_selector) {
            // Load the token ID from calldata.
            uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e721_safe_transfer_from_2_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);
        } else if (selector == e721_transfer_from_selector) {
            // Load the token ID from calldata.
            uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e721_transfer_from_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);
        } else if (selector == e721_approve_selector) {
            // Load the token ID from calldata.
            uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e721_approve_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);
        } else if (selector == e1155_safe_transfer_from_selector) {
            // Load the token ID from calldata.
            uint256 tokenId = uint256(
                _loadValueFromCalldata(data, e1155_safe_transfer_from_token_id_offset)
            );

            // Check if the selector is allowed.
            _revertSelectorOnActiveRental(selector, from, to, tokenId);
        } else if (selector == gnosis_safe_enable_module_selector) {
            // Load the extension address from calldata.
            address extension = address(
                uint160(
                    uint256(
                        _loadValueFromCalldata(data, gnosis_safe_enable_module_offset)
                    )
                )
            );

            // Check if the extension is whitelisted.
            _revertNonWhitelistedExtension(extension);
        } else if (selector == gnosis_safe_disable_module_selector) {
            // Load the extension address from calldata.
            address extension = address(
                uint160(
                    uint256(
                        _loadValueFromCalldata(data, gnosis_safe_disable_module_offset)
                    )
                )
            );

            // Check if the extension is whitelisted.
            _revertNonWhitelistedExtension(extension);
        } else {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L205-L267

## [G-15] Use hardcode address instead address(this)

Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this.

References:

https://book.getfoundry.sh/reference/forge-std/compute-create-address https://twitter.com/transmissions11/status/1518507047943245824


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol

90    abi.encodePacked(create2_ff, address(this), salt, keccak256(initCode))

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L90


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

402  uint256 trueBalance = IERC20(token).balanceOf(address(this));

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L402

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol

23  original = address(this);

33  IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier);

44   address(this),

73   if (address(this) == original) {

80   if (address(this) != rentalOrder.rentalWallet) {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L23


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol

124   ISafe(address(this)).enableModule(_stopPolicy);

127   ISafe(address(this)).setGuard(_guardPolicy);
 
163   address(this),

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124


## [G-16] Using assembly to revert with an error message

Issue Description - When reverting in solidity code, it is common practice to use a require or revert statement to revert execution with an error message. This can in most cases be further optimized by using assembly to revert with the error message.

Estimated Gas Savings - Here’s an example:

```solidity
/// calling restrictedAction(2) with a non-owner address: 24042
contract SolidityRevert {
    address owner;
    uint256 specialNumber = 1;
    constructor() {
        owner = msg.sender;
    }
    function restrictedAction(uint256 num)  external {
        require(owner == msg.sender, "caller is not owner");
        specialNumber = num;
    }
}
/// calling restrictedAction(2) with a non-owner address: 23734
contract AssemblyRevert {
    address owner;
    uint256 specialNumber = 1;
    constructor() {
        owner = msg.sender;
    }
    function restrictedAction(uint256 num)  external {
        assembly {
            if sub(caller(), sload(owner.slot)) {
                mstore(0x00, 0x20) // store offset to where length of revert message is stored
                mstore(0x20, 0x13) // store length (19)
                mstore(0x40, 0x63616c6c6572206973206e6f74206f776e657200000000000000000000000000) // store hex representation of message
                revert(0x00, 0x60) // revert with data
            }
        }
        specialNumber = num;
    }
}
```
From the example above, we can see that we get a gas saving of over 300 gas when reverting with the same error message with assembly against doing so in solidity. This gas savings come from the memory expansion costs and extra type checks the solidity compiler does under the hood.

Code Snippets:

```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol

38    revert Errors.Create2Deployer_UnauthorizedSender(msg.sender, salt);

46    revert Errors.Create2Deployer_AlreadyDeployed(targetDeploymentAddress, salt);

68    revert Errors.Create2Deployer_MismatchedDeploymentAddress(
                targetDeploymentAddress,
                deploymentAddress
            );
        
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L38


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

42    revert Errors.KernelAdapter_OnlyKernel(msg.sender);

79    revert Errors.Module_PolicyNotAuthorized(msg.sender);

133    revert Errors.Policy_OnlyRole(role);

183   revert Errors.Policy_ModuleDoesNotExist(keycode_);

255   if (msg.sender != executor) revert Errors.Kernel_OnlyExecutor(msg.sender);

163   if (msg.sender != admin) revert Errors.Kernel_OnlyAdmin(msg.sender);

313   revert Errors.Kernel_AddressAlreadyHasRole(addr_, role_);

335   if (!isRole[role_]) revert Errors.Kernel_RoleDoesNotExist(role_);

339   revert Errors.Kernel_AddressDoesNotHaveRole(addr_, role_);

362   revert Errors.Kernel_ModuleAlreadyInstalled(keycode);

393   revert Errors.Kernel_InvalidModuleUpgrade(keycode)

421   revert Errors.Kernel_PolicyAlreadyApproved(address(policy_));

458   if (!policy_.isActive()) revert Errors.Kernel_PolicyNotApproved(address(policy_));

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L42


```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

416    revert Errors.PaymentEscrowModule_PaymentTransferFailed(token, to, value);

279    revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));

367    revert Errors.PaymentEscrow_ZeroPayment();

383      revert Errors.PaymentEscrow_InvalidFeeNumerator();
 
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L416


```solidity
file:  blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol

222    revert Errors.StorageModule_OrderDoesNotExist(orderHash);

252    revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);

296    if (to.code.length == 0) revert Errors.StorageModule_NotContract(to);

299    if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);

318    if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);

322    revert Errors.StorageModule_InvalidHookStatusBitmap(bitmap);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L222


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol

74   revert Errors.ReclaimerPackage_OnlyDelegateCallAllowed();

81   revert Errors.ReclaimerPackage_OnlyRentalSafeAllowed(

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L74


```solidity
file: blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

202   revert Errors.CreatePolicy_OfferCountZero();

223   revert Errors.CreatePolicy_SeaportItemTypeNotSupported(offer.itemType);

297   revert Errors.CreatePolicy_SeaportItemTypeNotSupported(offer.itemType);

312   revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments);

333   revert Errors.CreatePolicy_ConsiderationCountZero();

343    revert Errors.CreatePolicy_SeaportItemTypeNotSupported(

389    revert Errors.CreatePolicy_SeaportItemTypeNotSupported(

397   revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments);

434   revert Errors.CreatePolicy_ConsiderationCountNonZero(

443   revert Errors.CreatePolicy_OfferCountNonZero(offers.length);

451  revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));

481   revert Errors.Shared_DisabledHook(target);

492  revert Errors.Shared_NonRentalHookItem(itemIndex);

506  revert Errors.Shared_HookFailString(revertReason);

512  revert Errors.Shared_HookFailString(

517  revert Errors.Shared_HookFailBytes(revertData);

632  revert Errors.CreatePolicy_RentDurationZero();

637  revert Errors.CreatePolicy_InvalidOrderMetadataHash();

659   revert Errors.CreatePolicy_InvalidRentalSafe(safe);

655  revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe);

767  revert Errors.CreatePolicy_UnauthorizedCreatePolicySigner();

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L202