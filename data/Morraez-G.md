Github Link: https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L428-L437

### Short Circuiting:
If `considerations.length > 0`, the contract should immediately revert. By reverting early in case of an error condition, the contract avoids wasting gas on unnecessary operations in `_processPayOrderOffer()`. This approach minimizes gas usage by avoiding needless operations if a precondition is not met.


```solidity
///////////////////////////////////////////////////////////////////////////
// Before
///////////////////////////////////////////////////////////////////////////
         
            // Process offer items.
            _processPayOrderOffer(items, offers, 0);

            // Assert that no consideration items are provided.
            if (considerations.length > 0) {
                revert Errors.CreatePolicy_ConsiderationCountNonZero(
                    considerations.length
                );
            }


///////////////////////////////////////////////////////////////////////////
// After
///////////////////////////////////////////////////////////////////////////   

            // Assert that no consideration items are provided.
            if (considerations.length > 0) {
                revert Errors.CreatePolicy_ConsiderationCountNonZero(
                    considerations.length
                );
            }

            // Process offer items.
            _processPayOrderOffer(items, offers, 0); 
```