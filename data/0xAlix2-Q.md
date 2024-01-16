Event may be emitted as the rental order has been stopped however it may still revert in the other functions ( ESCRW.settlePaymentBatch(orders); or         STORE.removeRentalsBatch(orderHashes, _convertToStatic(rentalAssetUpdates)); )

Any user that is using events to execute something ( on-chain or off-chain ) may think that the stopRent was successful.


https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L356