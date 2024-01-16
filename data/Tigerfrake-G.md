# [G-1] Splitting require()/if() statements that use && saves gas.

### Description:
This [issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by 13 gas.

### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2Fpolicies%2FStop.sol#L148

# [G-2] For Loop Optimization

### Description: 
The for-loop can be optimised in 4 ways:
• Removing initialization of loop counter if value is 0 by default.
• Caching array length outside the loop.
• Prefix increment (++i) instead of postfix increment (i++).
• Unchecked increment.

### Instances:
- https://github.com/re-nft/smart-contracts/blob/main/src%2Fmodules%2FStorage.sol#L229-L234
- https://github.com/re-nft/smart-contracts/blob/main/src%2Fmodules%2FStorage.sol#L249-L257

# [G-3] Switching between 1 and 2 instead of 0 and 1 (or false and true) is more gas efficient

### Description:
`SSTORE` from 0 to 1 (or any non-zero value) costs 20000 gas. `SSTORE` from 1 to 2 (or any other non-zero value) costs 5000 gas.
By storing the original value once again, a refund is triggered ( https://eips.ethereum.org/EIPS/eip-2200).
Since refunds are capped to a percentage of the total transaction’s gas, it is best to keep them low, to increase the likelihood of the full refund coming into effect.
Therefore, `switching between 1, 2 instead of 0, 1 will be more gas efficient`.
See: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/86bd4d73896afcb35a205456e361436701823c7a/contracts/security/ReentrancyGuard.sol#L29-L33

### Instance:
https://github.com/re-nft/smart-contracts/blob/main/src%2FCreate2Deployer.sol#L50