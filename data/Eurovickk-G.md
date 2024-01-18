https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol
GAS

1)Consider using transferFrom instead of transfer in _safeTransfer.
function _safeTransfer(address token, address to, uint256 value) internal {
    IERC20(token).transferFrom(msg.sender, to, value);
}
