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