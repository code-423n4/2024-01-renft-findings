# QA Report

## Low Severity Issues

### L-01 - The protocol should use a pull-over-push strategy for ERC-20 tokens to prevent reverts on rent stop

#### Impact

Reverts on ERC-20 tokens transfer will lead to reverts on the whole stop rent function.

This can be due to one of recipients is blacklisted by the token, or the token is paused for example.

Not only this affects the ERC-20 token transfers, but it means that NFTs will also remain locked, as they are transfered on the same function call.

#### Proof of Concept

[`stopRent()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L265) and [`stopRentBatch()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L313) are used to stop rents.

[Both NFTs reclaim and ERC-20 payment settlements](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L292-L296) are performed on the same transaction call, and there is no otherway of performing those actions.

This means that a revert on any of these functions will make the whole stop rent functionality revert, locking the assets.

#### Recommended Mitigation Steps

Implement a pull-over-push strategy for ERC-20 tokens. Instead of attempting to transfer them on a rent stop, store the balances that should be withdrawable by the different actors, and let them withdraw the balance on a separate transaction call.

### L-02 - It is possible to fulfill `PAY` orders with a `PAYEE` counterpart that doesn't fulfill via the protocol Zone

#### Impact

Broken developers assumptions that `PAYEE` counterparts are always validated via `Create::validateOrder()` since there's a good amount of code for validating `PAYEE` orders. 

[All of this code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L360-L399) and also [this code](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L439-L448) is useless, as it can be completely by passed.

It may also be possible to fulfill `BASE` orders with other orders that are not validated via the Zone (but isn't checked on the POC).

Such omission could lead to some critical issues by exploiting a vulnerable path. Raising to the protocol for awareness.

#### Proof of Concept

The following tests shows how a `PAY` order can be fulfilled with another order that isn't checked via the Zone `Create::validateOrder()` function.

It doesn't even need to be of type `PAYEE`, as it never enters the validation. Just keeping it because it's easier to create the test without modifying much more extra code.

The counterpart order is created with `FULL_OPEN` instead of the restricted one that forces the order to be validated against the Zone.

First add this code to `smart-contracts/test/fixtures/engine/OrderCreator.sol`. It changes the `orderType` to `FULL_OPEN` just for the `PAYEE` order that an adversary can create:

```diff
    function createOrder(
        ProtocolAccount memory offerer,
        OrderType orderType,
        uint256 erc721Offers,
        uint256 erc1155Offers,
        uint256 erc20Offers,
        uint256 erc721Considerations,
        uint256 erc1155Considerations,
        uint256 erc20Considerations
    ) internal {
+        if (orderType == OrderType.PAYEE) {
+            OrderComponentsLib
+                .empty()
+                .withOrderType(SeaportOrderType.FULL_OPEN) // @audit changed to `FULL_OPEN`
+                .withZone(address(0))                      // @audit no zone
+                .withStartTime(block.timestamp)
+                .withEndTime(block.timestamp + 100)
+                .withSalt(123456789)
+                .withConduitKey(conduitKey)
+                .saveDefault(STANDARD_ORDER_COMPONENTS);
+        }
```

Add this test to `smart-contracts/test/integration/Rent.t.sol`:

```solidity
    function test_Rent_PayOrder_Payee_FulfillingOutsideZone() public {
        // create a PAY order
        createOrder({
            offerer: alice,
            orderType: OrderType.PAY,
            erc721Offers: 1,
            erc1155Offers: 0,
            erc20Offers: 1,
            erc721Considerations: 0,
            erc1155Considerations: 0,
            erc20Considerations: 0
        });

        // finalize the pay order creation
        (
            Order memory payOrder,
            bytes32 payOrderHash,
            OrderMetadata memory payOrderMetadata
        ) = finalizeOrder();

        // create a PAYEE order
        createOrder({
            offerer: bob,
            orderType: OrderType.PAYEE,
            erc721Offers: 0,
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 1,
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the pay order creation
        (
            Order memory payeeOrder,
            bytes32 payeeOrderHash,
            OrderMetadata memory payeeOrderMetadata
        ) = finalizeOrder();

        // create an order fulfillment for the pay order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payOrder,
            orderHash: payOrderHash,
            metadata: payOrderMetadata
        });

        // create an order fulfillment for the payee order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payeeOrder,
            orderHash: payeeOrderHash,
            metadata: payeeOrderMetadata
        });

        // define the offer and consideration components for the ERC721
        FulfillmentComponent[] memory offerCompERC721 = new FulfillmentComponent[](1);
        FulfillmentComponent[] memory considCompERC721 = new FulfillmentComponent[](1);

        // link the ERC721 offer item in the PAY order to the ERC721 consideration item
        // in the PAYEE order
        offerCompERC721[0] = FulfillmentComponent({orderIndex: 0, itemIndex: 0});
        considCompERC721[0] = FulfillmentComponent({orderIndex: 1, itemIndex: 0});

        // define the offer and consideration components for the ERC20
        FulfillmentComponent[] memory offerCompERC20 = new FulfillmentComponent[](1);
        FulfillmentComponent[] memory considCompERC20 = new FulfillmentComponent[](1);

        // link the ERC20 offer item in the PAY order to the ERC20 consideration item
        // in the PAYEE order
        offerCompERC20[0] = FulfillmentComponent({orderIndex: 0, itemIndex: 1});
        considCompERC20[0] = FulfillmentComponent({orderIndex: 1, itemIndex: 1});

        // add the fulfillments to the order
        withSeaportMatchOrderFulfillment(
            Fulfillment({
                offerComponents: offerCompERC721,
                considerationComponents: considCompERC721
            })
        );
        withSeaportMatchOrderFulfillment(
            Fulfillment({
                offerComponents: offerCompERC20,
                considerationComponents: considCompERC20
            })
        );

        finalizePayOrderFulfillment();
    }
```

#### Recommended Mitigation

Fixing this on the `Create` policy would involve to have some state shared between subsequent "OpenSea" fulfillments (`PAY` + `PAYEE`). It may be easier to implement on the OpenSea fork: checking on all the fulfiller functions that both the `PAY` and `PAYEE` counterpart are present on the fulfillments.

### L-03 - `PaymentEscrow::_safeTransfer()` doesn't check for contract code size

[`_safeTransfer()`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L100-L118) doesn't check if the token contract has code, and no other part of the protocol does.

That allows an EOA to be passed as a token, and the transfer will succeed with no response (which is valid in the case of this function).

This opens an attack vector where tokens yet-to-be-deployed with known pre-computed addresses (like bridged tokens) can be utilized without making any real transfer. Then, when the token is created, the attacker would have balance of that token in the platform and would be able to operate with it.

The possible attack is mitigated externally on [SeaPort](https://github.com/ProjectOpenSea/seaport-core/blob/d4e8c74adc472b311ab64b5c9f9757b5bba57a15/src/lib/TokenTransferrer.sol#L235-L243), as it checks for the contract code, so no rentals should be able to be created with such tokens.

#### Recommended Mitigation

It is recommended to have the proper checks on the platform, and not rely on external integrations. 

Perform a contract check, like [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.5/contracts/utils/Address.sol#L202-L206) does for their SafeToken implementations.

### L-04 - The _DOMAIN_SEPARATOR should be recalculated in case of forks

The `_DOMAIN_SEPARATOR` in the `Signer` [is an `immutable`](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L33) and [calculated](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L273-L288) on the constructor with the current chain id.

In case of a chain fork, the corresponding chain id will not match, leading to the protocol not working on the forked chain.

Consider recalculating the `_DOMAIN_SEPARATOR` if the chain id changes