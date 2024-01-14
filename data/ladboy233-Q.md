| # | Issue Title |
|---|-------------|
| 1 | Renter Can Lock or Steal Lender's NFT if the NFT Used is Moonbird |
| 2 | Should Name the Function that Withdraws the Fee Explicitly |
| 3 | Renter Can Claim NFT Airdrop on Behalf of Original Lender |
| 4 | If the Recipient Is Not Capable of Receiving NFT, Both NFTs Are Locked |
| 5 | Does Not Support ETH Native Asset as Payment |
| 6 | Renter Can Still Transfer Out if the NFT Uses ERC1155A Standard |
| 7 | Loss of Rebase Token in Payment Escrow |
| 8 | Low Level Function Call Does Not Check Token Existence When Settle the Payment |

# Renter can lock or steal lender's nft if the nft used is moonbird

at the time of writing the bug report

moonbird nft, as one of the blue chip nft in last crypto bull run

https://opensea.io/collection/proof-moonbirds

has 343385 ETH trading volume

if the nft during the rental is moonbird,

the rental can maliciously prevent the lender from getting his nft back

after the rental period, the expect call flow is [transferring nft back](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L293)

```solidity
// Interaction: Transfer rentals from the renter back to lender.
_reclaimRentedItems(order);
```

this would eventually calls

```solidity
// Transfer each item if it is a rented asset.
for (uint256 i = 0; i < itemCount; ++i) {
	Item memory item = rentalOrder.items[i];

	// Check if the item is an ERC721.
	if (item.itemType == ItemType.ERC721)
		_transferERC721(item, rentalOrder.lender);

	// check if the item is an ERC1155.
	if (item.itemType == ItemType.ERC1155)
		_transferERC1155(item, rentalOrder.lender);
}
```

but if the nft is moonbird, during the rental period

the owner can call the function toggleNesting

https://etherscan.io/token/0x23581767a106ae21c074b2276d25e5c3e136a68b#code#F1#L319

and once toggleNesting is enabled, the nft transfer is disabled, 

meaning once the owner turn on the nesting, the original lender cannot transfer nft out

https://etherscan.io/token/0x23581767a106ae21c074b2276d25e5c3e136a68b#code#F1#L272

```solidity
    function _beforeTokenTransfers(
        address,
        address,
        uint256 startTokenId,
        uint256 quantity
    ) internal view override {
        uint256 tokenId = startTokenId;
        for (uint256 end = tokenId + quantity; tokenId < end; ++tokenId) {
            require(
                nestingStarted[tokenId] == 0 || nestingTransfer == 2,
                "Moonbirds: nesting"
            );
        }
    }
```

the function that can transfer nft out is 

https://etherscan.io/token/0x23581767a106ae21c074b2276d25e5c3e136a68b#code#F1#L258

safeTransferWhileNesting

in fact, the renter can steal lender's nft and bypass the guard check

the renter can just batch transaction via safe walllet by calling 

1. toggleNesting
2. safeTransferWhileNesting


# Should name the function that withdraw the fee explicitly

if the admin wants to withdraw the fee, the admin has to call the function [skim](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L164)

```solidity
    /**
     * @notice Skims all protocol fees from the escrow module to the target address.
     *
     * @param token Token address which denominates the fee.
     * @param to    Destination address to send the tokens.
     */
    function skim(address token, address to) external onlyRole("ADMIN_ADMIN") {
        ESCRW.skim(token, to);
    }
```

it is recommend rename the function to a more explicit name such as "withdrawFee" instead of name the function is "skim"

# Renter can claim nft airdrop onbehalf of original lender

when the order is fulfilled, the payment goes to payment escrow while the nft goes to the safe wallet

but during the rental term

if there are airdrop based on nft, the airdrop goes to the safe wallet

the owner of the safe wallet, which is renter, can claim the airdrop before lender reclaim the nft ownership.

# if the recipient is not capable of receiving nft, both nft are locked

when the rental term ends, 

the function call that transfer the nft back to the original lender is 

# does not support ETH native asset as payment

the protocol support ERC721 token, ERC1155 token, and ERC20 in offer and consideration

but in seaport protocol

if the asset field is address(0), it indicate the order can be settled with native ETH token, 

it is recommend that the renft protocol also support order settlement in native ETH token

# Renter can still transfer out if the nft use ERC1155A stand

once the nft is in the safe wallet, no one should transfert the nft out

but if the nft inherit and use ERC1155A standard

the owner can craft payload to call

https://github.com/superform-xyz/ERC1155A/blob/f46fa542026b860717d48bd1c09acbb8b68a0b57/src/interfaces/IERC1155A.sol#L96

```solidity
    /// @notice Public function for setting single id approval
    /// @dev Notice `owner` param, it will always be msg.sender, see _setApprovalForOne()
    /// @param spender address of the contract to approve
    /// @param id id of the ERC1155A to approve
    /// @param amount amount of the ERC1155A to approve
    function setApprovalForOne(address spender, uint256 id, uint256 amount) external;

    /// @notice Public function for setting multiple id approval
    /// @dev extension of sigle id approval
    /// @param spender address of the contract to approve
    /// @param ids ids of the ERC1155A to approve
    /// @param amounts amounts of the ERC1155A to approve
    function setApprovalForMany(address spender, uint256[] memory ids, uint256[] memory amounts) external;

    /// @notice Public function for increasing single id approval amount
    /// @dev Re-adapted from ERC20
    /// @param spender address of the contract to approve
    /// @param id id of the ERC1155A to approve
    /// @param addedValue amount of the allowance to increase by 
    function increaseAllowance(address spender, uint256 id, uint256 addedValue) external returns (bool);
```

to call setApprovalForOne, setApprovalForMany or increaseAllowance to approve external contract, then control external contract to transfer nft out

superform nft is one of the nft that use ERC1155A standard

# loss of rebase token in payment escrow

if the token has rebase behavior such as stETH or USDY, and if there is positive rebase

the additional rebased token is locked in the payment escrow contract and lost

# Low level function call does not check token existence when settle the payment

when the payment escrow settle token transfer, the function uses low level call

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L102

```solidity
    function _safeTransfer(address token, address to, uint256 value) internal {
        // Call transfer() on the token.
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(IERC20.transfer.selector, to, value)
        );

        // Because both reverting and returning false are allowed by the ERC20 standard
        // to indicate a failed transfer, we must handle both cases.
        //
        // If success is false, the ERC20 contract reverted.
        //
        // If success is true, we must check if return data was provided. If no return
        // data is provided, then no revert occurred. But, if return data is provided,
        // then it must be decoded into a bool which will indicate the success of the
        // transfer.
        if (!success || (data.length != 0 && !abi.decode(data, (bool)))) {
            revert Errors.PaymentEscrowModule_PaymentTransferFailed(token, to, value);
        }
    }
```

but if the token is self-destructed and no longer exists, no token transfer is done but the payment is still settled

it is recommend to use openzeppelin safeTransfer, it has built-in contract existence check by default





