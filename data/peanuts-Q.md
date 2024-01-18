### [L-01] Setting fees in payment escrow should have an max limit

The admin can set a fee of 10000, which is 100% of the payment

```
    /**
     * @notice Sets the numerator for the fee. The denominator will always be set at
     *         10,000.
     *
     * @param feeNumerator Numerator of the fee.
     */
    function setFee(uint256 feeNumerator) external onlyByProxy permissioned {
        // Cannot accept a fee numerator greater than 10000.
>       if (feeNumerator > 10000) {
            revert Errors.PaymentEscrow_InvalidFeeNumerator();
        }


        // Set the fee.
        fee = feeNumerator;
    }
```

There should be a max upper limit cap of setting fees, like `feeNumerator > 1000` (10%) max fees.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L380-L387

### [L-02] Threshold and owners parameters is double-checked

When deploying a rental safe in Factory.sol, the owners and threshold is checked.

```
    function deployRentalSafe(
        address[] calldata owners,
        uint256 threshold
    ) external returns (address safe) {
        // Require that the threshold is valid.
        if (threshold == 0 || threshold > owners.length) {
            revert Errors.FactoryPolicy_InvalidSafeThreshold(threshold, owners.length);
        }

```

This check is not needed as the safe contract itself, in `ISafe.setup()`, the function checks the owner and threshold.

```
    function setupOwners(address[] memory _owners, uint256 _threshold) internal {
        // Threshold can only be 0 at initialization.
        // Check ensures that setup function can only be called once.
        require(threshold == 0, "GS200");
        // Validate that threshold is smaller than number of added owners.
        require(_threshold <= _owners.length, "GS201");
```


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L138-L145


### [L-03] PaymentEscrow.safetransfer doesn't check that the token.code.length is greater than 0

In SafeERC20, callOptionalReturn checks success, returndata length and token.code.length.

```
    function _callOptionalReturnBool(IERC20 token, bytes memory data) private returns (bool) {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We cannot use {Address-functionCall} here since this should return false
        // and not revert is the subcall reverts.


        (bool success, bytes memory returndata) = address(token).call(data);
>       return success && (returndata.length == 0 || abi.decode(returndata, (bool))) && address(token).code.length > 0;
    }
```

In PaymentEscrow, the `address(token).code.length` is not checked.

```
    function _safeTransfer(address token, address to, uint256 value) internal {
        // Call transfer() on the token.
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(IERC20.transfer.selector, to, value)
        );
        if (!success || (data.length != 0 && !abi.decode(data, (bool)))) {
            revert Errors.PaymentEscrowModule_PaymentTransferFailed(token, to, value);
        }
```

Check the address(token).code.length as well.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L115-L118

https://github.com/OpenZeppelin/openzeppelin-contracts/blob/b27cd83eba9f181f183c78ed85df62712d2852bf/contracts/token/ERC20/utils/SafeERC20.sol#L110-L117


### [L-04] Safe doesn't allow safeTransfers, which will break interactions when a protocol safemints a token 

There can be many use cases for renting NFTs. For example, a renter can rent an NFT and use it to play a game. In web3 games, there might be instances where the game project will mint NFTs to the user if they have the NFTs required. Sometimes, they use safeMint, or even safeTransfers.

The Gnosis safe does not have onERC721Received implemented. Thus, they cannot receive any form of tokens when safeMint or safeTransfers are called.

Have an extended policy (Like Stop or Guard) to ensure that safeTransfers and safeMints work for rental assets.

https://ethereum.stackexchange.com/questions/124516/safetransferfrom-of-erc721-not-working-when-transfering-nft-to-a-safe-address