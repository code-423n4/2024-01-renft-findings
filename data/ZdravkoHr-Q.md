# [LOW-1] `PUSH0` is not supported on some chains
The protocol will be deployed on all chains that are supported by Safe. This includes chains like Polygon where the `PUSH0` opcode is not supported. The current pragma of the contracts `^0.8.20` allows compiling with 0.8.20 which uses PUSH0. If the contracts are compiled with this compiler, they will not work on these chains after deployment.

# [LOW-2] `_calculateFee` rounds in favour of users, and not of the protocol
The [Escrow Module](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol) keeps the ERC20 tokens during an active rental that have to be paid to a certain party. When the rent is stopped, the tokens are transferred to the recipient and a small fee is taken for the protocol. The issue is that the protocol does not round in its own favour but in users' in [_calculateFee](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88C1-L91C6). 

```solidity
    function _calculateFee(uint256 amount) internal view returns (uint256) {
        // Uses 10,000 as a denominator for the fee.
        return (amount * fee) / 10000;
    }
```

Since any ERC20 tokens are in scope of this audit, if the ERC20 is with little decimals, for example `3`, the loss will be of importance. 

For instance:
`120234 * 1000 / 10000`

120.234 tokens with fee of 10%. The fee taken should be 12.0234, but it will be 12.023, which is a loss of 0.0004 tokens for this example.

My recommendation is to round in your favour:
```diff
    function _calculateFee(uint256 amount) internal view returns (uint256) {
        // Uses 10,000 as a denominator for the fee.
  -      return (amount * fee) / 10000;
  +      return ((amount * fee) + 5000) / 10000);
    }
```

# [LOW-3] A risk of transferring locked NFTs if the `Stop Policy` is enabled as a whitelisted delegate address

The [Stop Policy](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol) will be enable as a Safe Module, then safes will delegate call to the policy which inherits from [Reclaimer.sol](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Reclaimer.sol) and that policy will call [`reclaimRentalOrder`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L71-L101). If the stop policy becomes an enabled delegate address, everyone will be able to call `reclaimRentalOrder` and move the locked NFTs.

# [LOW-4] The contracts may stop working if a reorg happens
If a reorg happens, the block.chainId will be changed. In [Signer.sol](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L64) the `CHAIN_ID` is set in the constructor and cannot be changed afterwards. In the case of reorg, there will be an incongruity between block.chainId and _CHAIN_ID and the whole Signer will not work. I recommend making _CHAIN_ID a storage variable (currently immutable) and adding a function that updates it to block.chainid

# [LOW-5] Allowed rental execution on deadline
[Signer._validateProtocolSignatureExpiration()]() allows executing rentals exactly on the deadline, when they should be already expired. Change that

```diff
   function _validateProtocolSignatureExpiration(uint256 expiration) internal view {
        // Check that the signature provided by the protocol signer has not expired.
        // @audit allows execution at expiration
-        if (block.timestamp > expiration) {
+        if (block.timestamp >= expiration) {
            revert Errors.SignerPackage_SignatureExpired(block.timestamp, expiration);
        }
    }
```
