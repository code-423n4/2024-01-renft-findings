# Not setting a maximum limit for the rental duration could result in locking the NFTs into the safe

## Impact

* Likelihood : LOW 
* Impact : HIGH

reNFT allows people to rent NFTs by fulfilling a Seaport order. An owner (lender), can choose to use the BASE method (earn a payout from renting his NFTs) or the PAY method (the renter receveives money by renting the NFTs).
The renter will be able to enjoy his rented NFTs, that are gonna placed in a safe (Gnosis) where he is not able to move them to another contract.

As the Safe can be valid for an unlimited period of time, if the rental duration is to long and you cant reclaim your NFTs before it's over (using the BASE method), the NFTs would remain stuck in the safe.
Lenders could not get them back and they would be still profitable for renters.

## Proof of Concept

In the function `Create::_rentFromZone` in https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L530 the endTimestamp is based on the payload.metadata.rentDuration from Seaport:

```
            // Generate the rental order.
            RentalOrder memory order = RentalOrder({
                seaportOrderHash: seaportPayload.orderHash,
                items: items,
                hooks: payload.metadata.hooks,
                orderType: payload.metadata.orderType,
                lender: seaportPayload.offerer,
                renter: payload.intendedFulfiller,
                rentalWallet: payload.fulfillment.recipient,
                startTimestamp: block.timestamp,
                endTimestamp: block.timestamp + payload.metadata.rentDuration
            });
```

The only check done is if this rentDuration is not equal to 0 in https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L631

```
    function _isValidOrderMetadata(
        OrderMetadata memory metadata,
        bytes32 zoneHash
    ) internal view {
        // Check that the rent duration specified is not zero.
        if (metadata.rentDuration == 0) {
            revert Errors.CreatePolicy_RentDurationZero();
        }
```

## Tools Used
Manual review (vscode).

## Recommended Mitigation Steps
It would be recommended to set a maximum reasonnable time limit for the rental duration in order to secure renters that rent using BASE method.
This maximum duration time could be extended if necessary.