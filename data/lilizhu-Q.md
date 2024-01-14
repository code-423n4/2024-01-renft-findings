# N-1: Constants should be defined rather than using magic numbers

**Even assembly can benefit from using readable constants instead of hex/numeric literals.**

**I suggest using** `constant` **variables as this would make the code more maintainable and readable while costing nothing gas-wise (constants are replaced by their value at compile-time).**

***There are 2 instances of this issue:***

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88C1-L91C6

```jsx
function _calculateFee(uint256 amount) internal view returns (uint256) {
        // Uses 10,000 as a denominator for the fee.
        return (amount * fee) / 10000;
    }

```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L132C1-L146C6
```jsx
function _calculatePaymentProRata(
        uint256 amount,
        uint256 elapsedTime,
        uint256 totalTime
    ) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {
        // Calculate the numerator and adjust by a multiple of 1000.
        uint256 numerator = (amount * elapsedTime) * 1000;

        // Calculate the result, but bump by 500 to add a rounding adjustment. Then,
        // reduce by a multiple of 1000.
        renterAmount = ((numerator / totalTime) + 500) / 1000;

        // Calculate lender amount from renter amount so no tokens are left behind.
        lenderAmount = amount - renterAmount;
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