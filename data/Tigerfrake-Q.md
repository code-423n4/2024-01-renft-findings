# [01] Unsafe downcasting from `uint256` to `uint160`

### Description:
Casting a `uint256` to a `uint160` in Solidity results in the loss of the `upper 96 bits` of the original `uint256` value. This operation can potentially lead to significant data loss if the `uint256` value exceeds the maximum value that can be represented by a `uint160`.

The calculated `address hash` in the `_getCreate2Address()` function is then cast to a `uint256` and then to a `uint160` to remove the `upper 96 bits`, and finally to an `address`.
However, as mentioned earlier, this last step involves a potential data loss if the `address hash` is larger than what can be represented by a `uint160`

### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2FCreate2Deployer.sol#L84-L95

### Recommendation:
> Use SafeCast library for downcasting

# [02] Violation of Function & Variable Naming Convention

### Description:
The linked variables/functions do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `underscore`.

### Instances:
- https://github.com/re-nft/smart-contracts/blob/main/src%2FKernel.sol#L90-L108
- https://github.com/re-nft/smart-contracts/blob/main/src%2Fpackages%2FSigner.sol#L29-L39

### Recommendation:
> Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the Solidity.

# [03] Undocumented Value Literal

### Description:
The value literal 10000 is meant to be used as a divisor in fee calculation in the `_calculatefee()` function. However, it is undocumented and unclearly depicted.

```Solidity
        return (amount * fee) / 10000;
```
### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2Fmodules%2FPaymentEscrow.sol#L90

### Recommendation:
> I advise the special underscore (_) separator to be applied to it (i.e. 10000 would become 10_000) 

# [04] Possible Unchecked Return Values.

### Description:
Several function calls within the contract do not check their return values. For instance, the `_installModule, _upgradeModule, _activatePolicy, _deactivatePolicy, _migrateKernel, _reconfigurePolicies, _setPolicyPermissions`, and `_pruneFromDependents` functions do not check whether the operations were successful or not. 

### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2FKernel.sol#L356-L531

### Recommendation:
> It would be better to add checks after these function calls to handle possible failures.

# [05] Lack of Events.

### Description:
The contracts emits `events` in some places but not everywhere it should. Emitting events can help external observers track the contract's activity, and it can also make debugging easier. 

### Recommendation:
> Consider adding events in places where they are missing.

# [06] Avoid Block Timestamp Manipulation

### Description:
`Block timestamps` have been used historically for a number of purposes, including `entropy for random numbers locking funds for a set amount of time, and different state-changing, time-dependent conditional statements`. 
Because validators have the capacity to slightly alter `timestamps`, using `block timestamps` wrong in smart contracts can be quite risky.
The time difference between events can be estimated using `block.number` and the average `block time`. However, because `block times` can change and break functionality, it's best to avoid its use.

### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2Fpolicies%2FStop.sol#L132

# [07] Unchecked External Calls

### Description:
External fucntion calls can introduce vulnerabilities when not properly validated. Trusting external calls without proper checks can lead to unintended behavior or even attacks.

Example: the `increaseDeposit()` function fully trusts that `_increaseDeposit()` function it calls within it will function as intended without any checks for its success

### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2Fmodules%2FPaymentEscrow.sol#L361-L372

### Recommendation:
> Always check the return value of external calls and handle any errors or exceptions. Implement checks to verify the integrity of your function calls.

# [08] For same condition checks, use modifiers

### Description:
The main advantage of using modifiers for the same condition checks in different functions is code reusability and readability. 
Instead of repeating the same condition check in every function that requires it, you can define a modifier once and then apply it to any function that needs it. This makes your code cleaner and easier to maintain.

### Instances:

- https://github.com/re-nft/smart-contracts/blob/main/src%2Fmodules%2FStorage.sol#L299
- https://github.com/re-nft/smart-contracts/blob/main/src%2Fmodules%2FStorage.sol#L318

# [09] Floating pragma

### Description:
The disadvantage of using a floating pragma is that it can lead to inconsistencies and potential bugs. When a contract is compiled with a different compiler version, it might produce different bytecode due to changes in the compiler, even if the source code looks the same. This can lead to unexpected behavior if the contract is deployed on a network with a different compiler version than the one it was tested with.

Moreover, if the contract uses features that were introduced in a newer compiler version, and it gets compiled with an older compiler version, it could fail or behave differently than expected.

### Instances:
All contracts
```Solidity
pragma solidity ^0.8.20;
```

### Recommendation:
> It's generally recommended to lock the compiler version to avoid these issues.
