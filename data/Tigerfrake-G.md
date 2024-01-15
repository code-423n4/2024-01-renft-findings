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
