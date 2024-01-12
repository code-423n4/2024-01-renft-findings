#### [GAS-01] `PaymentEscrow::skim` can be improved

`syncedBalance` is used only once, it can be unpacked in `skimmedBalance` calculation below.

```
    function skim(address token, address to) external onlyByProxy permissioned {
        // Fetch the currently synced balance of the escrow.
--      uint256 syncedBalance = balanceOf[token];

        // Fetch the true token balance of the escrow.
        uint256 trueBalance = IERC20(token).balanceOf(address(this));

        // Calculate the amount to skim.
--      uint256 skimmedBalance = trueBalance - syncedBalance;
++      uint256 skimmedBalance = trueBalance - balanceOf[token];

        // Send the difference to the specified address.
        _safeTransfer(token, to, skimmedBalance);

        // Emit event with fees taken.
        emit Events.FeeTaken(token, skimmedBalance);
    }
```

#### [GAS-02] `PaymentEscrow::_calculatePaymentProRata` can be improved

`numerator` is used only once, it can be unpacked in `renterAmount` calculation.

```
    function _calculatePaymentProRata(
        uint256 amount,
        uint256 elapsedTime,
        uint256 totalTime
    ) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {
        // Calculate the numerator and adjust by a multiple of 1000.
--      uint256 numerator = (amount * elapsedTime) * 1000;

        // Calculate the result, but bump by 500 to add a rounding adjustment. Then,
        // reduce by a multiple of 1000.
--      renterAmount = ((numerator / totalTime) + 500) / 1000;
++      renterAmount = (((amount * elapsedTime) * 1000 / totalTime) + 500) / 1000;

        // Calculate lender amount from renter amount so no tokens are left behind.
        lenderAmount = amount - renterAmount;
    }
```