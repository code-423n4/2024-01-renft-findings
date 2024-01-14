# N-1: Constants should be defined rather than using magic numbers

**Even assembly can benefit from using readable constants instead of hex/numeric literals.**

**I suggest using** `constant` **variables as this would make the code more maintainable and readable while costing nothing gas-wise (constants are replaced by their value at compile-time).**

***There are 1 instances of this issue:***

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88C1-L91C6

```jsx
function _calculateFee(uint256 amount) internal view returns (uint256) {
        // Uses 10,000 as a denominator for the fee.
        return (amount * fee) / 10000;
    }

```

## Tools Used

None

## Recommended Mitigation Steps

```solidity
// the MAX_FEE_NUMERATOR a denominator for the fee
 uint256 public immutable MAX_FEE_NUMERATOR = 10000;

function _calculateFee(uint256 amount) internal view returns (uint256) {
        // Uses 10,000 as a denominator for the fee.
        return (amount * fee) / MAX_FEE_NUMERATOR;
    }

```