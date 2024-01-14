

## L-2: Unsafe ERC20 Operations should not be used

ERC20 functions may not behave as expected. For example: return values are not always meaningful. It is recommended to use OpenZeppelin's SafeERC20 library.

- Found in src/examples/revenue-share/ERC20RewardHook.sol [Line: 215](src/examples/revenue-share/ERC20RewardHook.sol#L215)

	```solidity
	        rewardToken.transfer(msg.sender, withdrawAmount);
	```

- Found in src/modules/PaymentEscrow.sol [Line: 103](src/modules/PaymentEscrow.sol#L103)

	```solidity
	            abi.encodeWithSelector(IERC20.transfer.selector, to, value)
	```