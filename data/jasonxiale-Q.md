## `Create._processPayOrderOffer` and `Create._processPayeeOrderConsideration` can be bypassed partially
File:
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L276-L286
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L218-L220
In `_processBaseOrderOffer` only check [offers.length](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L201-L203) shoulde be larger than 0, if the offer type is ERC1155, the function doesn't **check if ERC1155's amount is larger than 0**. If the offers in `Create._processBaseOrderOffer` are all ERC115 type, and **0** as amount, it acts like [offers.length == 0](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L201-L203), and should revert
Similar issue happens in [Create._processPayOrderOffer](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L276-L284) too.

## if `Stop.stopRent` or `stop.stopRentBatch` is not called, after the rental duration, the renter can continue use the lender's token for free
File:
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L313-L365
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265-L306
In current implementation, after the rential duration, the ERC721/ERC1155 will be owned by the renter if `Stop.stopRent/stopRentBatch` is not called, this might be an issue because the renter can use the assets for free