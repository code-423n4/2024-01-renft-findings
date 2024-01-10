# [G-3] Splitting require()/if() statements that use && saves gas.

### Description:
This [issue](https://github.com/code-423n4/2022-01-xdefi-findings/issues/128) describes the fact that there is a larger deployment gas cost, but with enough runtime calls, the change ends up being cheaper by 13 gas.

### Instances:
https://github.com/re-nft/smart-contracts/blob/main/src%2Fpolicies%2FStop.sol#L148