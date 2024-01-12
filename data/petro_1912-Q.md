- Unnecessary abstract contract `Accumulator` and the call to its function.
- Gas Optimization

`Create Policy` contract and `Stop Policy` contract has 2 functions(`_insert`, and `_convertStatic`) from `Accumulator` abstract contract.


While creating and stopping rents, we need to add or remove rental items in offer and consideration from `Storage Module`.

We can't know a number of rental items in offer and consideration without a loop, so that can't use static array. 
In order to avoid to use a dynamic array, `Create and Stop Policy` contracts pack only rental `items` (itemType is ERC721 or ERC1155) to `bytes` type by calling `_insert` function, and then convert bytes to an array of `RentalAssetUpdate` struct by calling `_convertStatic` function.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L533-L617
`_rentFromZone` function in  Create.sol
```
            bytes memory rentalAssetUpdates = new bytes(0);

            // Check if each item is a rental. If so, then generate the rental asset update.
            // Memory will become safe again after this block.
567:        for (uint256 i; i < items.length; ++i) {
568:            if (items[i].isRental()) {
569:                // Insert the rental asset update into the dynamic array.
570:                _insert(
571:                    rentalAssetUpdates,
572:                    items[i].toRentalId(payload.fulfillment.recipient),
573:                    items[i].amount
574:                );
575:            }
576:        }

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

            // Compute the order hash.
            bytes32 orderHash = _deriveRentalOrderHash(order);

            // Interaction: Update storage only if the order is a Base Order or Pay order.
595:        STORE.addRentals(orderHash, _convertToStatic(rentalAssetUpdates));
```

I think such processes are necessary, we just pass `items` or `orders` directly to `addRentals`, `removeRentals`, `removeRentalsBatch` functions.  

So we can remove lines L567-L576 and rewrite L596 like this.
```
STORE.addRentals(orderHash, items, payload.fulfillment.recipient);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L189-L204
Original `addRentals` function in `Storage Module` contract.
```
function addRentals(
        bytes32 orderHash,
        RentalAssetUpdate[] memory rentalAssetUpdates
) external onlyByProxy permissioned {
        // Add the order to storage.
        orders[orderHash] = true;

        // Add the rented items to storage.
        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Update the order hash for that item.
            rentedAssets[asset.rentalId] += asset.amount;
    }
}
```

We can rewrite this function like the following:
```
using RentalUtils for Item;

function addRentals(
        bytes32 orderHash,
        Item[] memory items,
        address receipient
) external onlyByProxy permissioned {
        // Add the order to storage.
        orders[orderHash] = true;

        // Add the rented items to storage.
        for (uint256 i = 0; i < items.length; ++i) {
            Item memory item = items[i];
            if (item.isRental()) {

                RentalId rentalId = item.toRentalId(receipient);
                rentedAssets[rentalId] += item.amount;
            }
        }
    }
}
```

Original `stopRent` and `stopRentBatch` in Stop.sol
```
    function stopRent(RentalOrder calldata order) external {
        // Check that the rental can be stopped.
        _validateRentalCanBeStoped(order.orderType, order.endTimestamp, order.lender);

        // Create an accumulator which will hold all of the rental asset updates, consisting of IDs and
        // the rented amount. From this point on, new memory cannot be safely allocated until the
        // accumulator no longer needs to include elements.
        bytes memory rentalAssetUpdates = new bytes(0);

        // Check if each item in the order is a rental. If so, then generate the rental asset update.
        // Memory will become safe again after this block.
276:    for (uint256 i; i < order.items.length; ++i) {
277:        if (order.items[i].isRental()) {
278:            // Insert the rental asset update into the dynamic array.
279:            _insert(
280:                rentalAssetUpdates,
281:                order.items[i].toRentalId(order.rentalWallet),
282:                order.items[i].amount
283:            );
284:        }
285:    }

        // Interaction: process hooks so they no longer exist for the renter.
        if (order.hooks.length > 0) {
            _removeHooks(order.hooks, order.items, order.rentalWallet);
        }

        // Interaction: Transfer rentals from the renter back to lender.
        _reclaimRentedItems(order);

        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
        ESCRW.settlePayment(order);

        // Interaction: Remove rentals from storage by computing the order hash.
299:    STORE.removeRentals(
300:        _deriveRentalOrderHash(order),
301:        _convertToStatic(rentalAssetUpdates)
302:    );

        // Emit rental order stopped.
        _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);
    }

    /**
     * @notice Stops a batch of rentals by providing an array of `RentalOrder` structs.
     *
     * @param orders Array of rental orders to stop.
     */
    function stopRentBatch(RentalOrder[] calldata orders) external {
        // Create an array of rental order hashes which will be removed from storage.
        bytes32[] memory orderHashes = new bytes32[](orders.length);

        // Create an accumulator which will hold all of the rental asset updates, consisting of IDs and
        // the rented amount. From this point on, new memory cannot be safely allocated until the
        // accumulator no longer needs to include elements.
        bytes memory rentalAssetUpdates = new bytes(0);

        // Process each rental order.
        // Memory will become safe after this block.
        for (uint256 i = 0; i < orders.length; ++i) {
            // Check that the rental can be stopped.
            _validateRentalCanBeStoped(
                orders[i].orderType,
                orders[i].endTimestamp,
                orders[i].lender
            );

            // Check if each item in the order is a rental. If so, then generate the rental asset update.
333:        for (uint256 j = 0; j < orders[i].items.length; ++j) {
334:            // Insert the rental asset update into the dynamic array.
335:            if (orders[i].items[j].isRental()) {
336:                _insert(
337:                    rentalAssetUpdates,
338:                    orders[i].items[j].toRentalId(orders[i].rentalWallet),
339:                        orders[i].items[j].amount
340:                );
341:            }
342:        }

            // Add the order hash to an array.
            orderHashes[i] = _deriveRentalOrderHash(orders[i]);

            // Interaction: Process hooks so they no longer exist for the renter.
            if (orders[i].hooks.length > 0) {
                _removeHooks(orders[i].hooks, orders[i].items, orders[i].rentalWallet);
            }

            // Interaction: Transfer rental assets from the renter back to lender.
            _reclaimRentedItems(orders[i]);

            // Emit rental order stopped.
            _emitRentalOrderStopped(orderHashes[i], msg.sender);
        }

        // Interaction: Transfer ERC20 payments from the escrow contract to the respective recipients.
        ESCRW.settlePaymentBatch(orders);

        // Interaction: Remove all rentals from storage.
363:    STORE.removeRentalsBatch(orderHashes, _convertToStatic(rentalAssetUpdates));
}
```

As `Create Policy`, We can remove lines(L276-L285, L333-L342), and rewrite L299-L302, and L363 in `Stop Policy` like below:

`Stop.sol`
```
299:    STORE.removeRentals(
300:        _deriveRentalOrderHash(order),
301:        order
303:    );
...
363:    STORE.removeRentalsBatch(orderHashes, orders);
```

`Storage.sol`
```
    function removeRentals(
        bytes32 orderHash,
        RentalOrder calldata order
    ) external onlyByProxy permissioned {
        // The order must exist to be deleted.
        if (!orders[orderHash]) {
            revert Errors.StorageModule_OrderDoesNotExist(orderHash);
        } else {
            // Delete the order from storage.
            delete orders[orderHash];
        }

        // Process each rental asset.
        uint256 itemsLength = order.items.length;
        for (uint256 i = 0; i < itemsLength; ++i) {
            Item memory item = order.items[i];
            if (item.isRental()) {
                RentalId rentalId = item.toRentalId(order.rentalWallet);

                // Reduce the amount of tokens for the particular rental ID.
                rentedAssets[rentalId] -= item.amount;
            }
        }
    }

    function removeRentalsBatch(
        bytes32[] calldata orderHashes,
        RentalOrder[] calldata orders
    ) external onlyByProxy permissioned {
        // Delete the orders from storage.
        for (uint256 i = 0; i < orderHashes.length; ++i) {
            // The order must exist to be deleted.
            if (!orders[orderHashes[i]]) {
                revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i]);
            } else {
                // Delete the order from storage.
                delete orders[orderHashes[i]];
            }
        }

        // Process each rental asset.
        for (uint256 i = 0; i < orders.length; ++i) {
            for (uint256 j = 0; j < orders[i].items.length; ++j) {
                // Insert the rental asset update into the dynamic array.
                Item memory item = orders[i].items[j];
                if (item.isRental()) {
                    RentalId rentalId = item.toRentalId(orders[i].rentalWallet);
                    rentedAssets[rentalId] -= item.amount;
                }
            }
        }
    }
```

And then We can remove `Accumulator.sol` and remove inheritance from it in `Create.sol` and `Stop.sol`
 