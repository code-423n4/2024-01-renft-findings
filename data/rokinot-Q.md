# Low

### Reversed orders can be created, locking both offers and considerations

During the process of creating an order, regardless if it's base or pay type, it's possible to switch the addresses of the ERC721 and ERC20 offer and consideration respectively. This leads to a situation where the ERC20 tokens are sent to the renter's address while the ERC721s are sent to the payment escrow. 

Here's a proof of concept, showing that the rental is created but cannot be stopped. I've reused a PoC of another submission, so in order to run this test you must fork the mainnet.

```solidity
pragma solidity ^0.8.22;

import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";
import {MockTarget} from "@test/mocks/MockTarget.sol";
import {
    AdvancedOrderLib,
    ConsiderationItemLib,
    FulfillmentComponentLib,
    FulfillmentLib,
    OfferItemLib,
    OrderComponentsLib,
    OrderLib,
    OrderParametersLib,
    SeaportArrays,
    ZoneParametersLib
} from "@seaport-sol/SeaportSol.sol";
import {
    ConsiderationItem,
    OfferItem,
    OrderParameters,
    OrderComponents,
    Order,
    AdvancedOrder,
    ItemType,
    CriteriaResolver,
    OrderType as SeaportOrderType
} from "@seaport-types/lib/ConsiderationStructs.sol";
import {OrderMetadata, OrderType, Hook, RentalOrder} from "@src/libraries/RentalStructs.sol";
import {OrderCreator} from "@test/fixtures/engine/OrderCreator.sol";
import {ProtocolAccount} from "@test/utils/Types.sol";
import {BaseTest} from "@test/BaseTest.sol";
import {SafeUtils} from "@test/utils/GnosisSafeUtils.sol";

import {IERC721} from "@openzeppelin-contracts/token/ERC721/IERC721.sol";

interface IEtherRockERC721 is IERC721 {
    function getRockInfo(uint tokenId) external view returns (address, bool, uint, uint);
    function checkIfUpdateRequired(uint tokenId) external view returns (bool updateRequired);
    function update(uint tokenId) external;
    function updateAll() external;
    function wrap(uint tokenId) external;
    function unwrap(uint tokenId) external;
}

interface IEtherRock {
    function getRockInfo (uint rockNumber) external returns (address, bool, uint, uint);
    function rockOwningHistory (address _address) external returns (uint[] calldata);
    function buyRock (uint rockNumber) payable external;
    function sellRock (uint rockNumber, uint price) external;
    function dontSellRock (uint rockNumber) external;
    function giftRock (uint rockNumber, address receiver) external;
    function withdraw() external;
}

// Tests functionality on the guard related to transaction checking
contract Guard_CheckTransaction_Unit_Test is BaseTest {
    using OfferItemLib for OfferItem;
    using ConsiderationItemLib for ConsiderationItem;
    using OrderComponentsLib for OrderComponents;
    using OrderLib for Order;
    using ECDSA for bytes32;

    IEtherRockERC721 wrappedrock;
    IEtherRock etherrock;

    function setUp() public override {
        super.setUp();

        OrderComponentsLib
            .empty()
            .withOrderType(SeaportOrderType.FULL_RESTRICTED)
            .withZone(address(create))
            .withStartTime(block.timestamp)
            .withEndTime(block.timestamp + 100)
            .withSalt(123456789)
            .withConduitKey(conduitKey)
            .saveDefault(STANDARD_ORDER_COMPONENTS);

        //0x41f28833Be34e6EDe3c58D1f597bef429861c4E2 etherrock
        //0xA3F5998047579334607c47a6a2889BF87A17Fc02 wrapped

        etherrock = IEtherRock(0x41f28833Be34e6EDe3c58D1f597bef429861c4E2);
        wrappedrock = IEtherRockERC721(0xA3F5998047579334607c47a6a2889BF87A17Fc02);

        (address ownerOf11, , uint256 price, ) = etherrock.getRockInfo(11);

        vm.deal(alice.addr, price);

        vm.startPrank(alice.addr);
        etherrock.buyRock{value: price}(11);
        wrappedrock.update(11);
        etherrock.giftRock(11, address(wrappedrock));
        wrappedrock.wrap(11);
        wrappedrock.approve(address(conduit), 11);
        vm.stopPrank();

        vm.deal(bob.addr, 1000000000000000000000);

        vm.startPrank(bob.addr);
        etherrock.buyRock{value: 1000000000000000000000}(12);
        wrappedrock.update(12);
        etherrock.giftRock(12, address(wrappedrock));
        wrappedrock.wrap(12);
        wrappedrock.approve(address(conduit), 12);
        vm.stopPrank();
    }

    function test_nftAsConsiderationForRental() public {
        //Alice, owner of the wrapped rock, creates a reverse order
        OfferItem memory offerItem;
        offerItem.itemType = ItemType.ERC20;
        offerItem.token = address(wrappedrock);
        offerItem.identifierOrCriteria = 0;
        offerItem.startAmount = 11;
        offerItem.endAmount = 11;

        OfferItem memory offerItem2;
        offerItem2.itemType = ItemType.ERC721;
        offerItem2.token = address(erc20s[0]);
        offerItem2.identifierOrCriteria = 100;
        offerItem2.startAmount = 1;
        offerItem2.endAmount = 1;

        OrderMetadata memory metadata;
        metadata.orderType = OrderType.PAY; 
        metadata.rentDuration = 500;
        metadata.emittedExtraData = new bytes(0);

        OrderCreator.OrderToCreate memory createPay;
        createPay.offerer = alice;
        createPay.offerItems = new OfferItem[](2);
        createPay.offerItems[0] = offerItem;
        createPay.offerItems[1] = offerItem2;
        createPay.metadata = metadata;

        (Order memory payOrder, bytes32 payOrderHash) = _createSignedOrderTest(
            createPay.offerer,
            createPay.offerItems,
            createPay.considerationItems,
            createPay.metadata
        );

        //bob fulfills
        ConsiderationItem memory consideration;
        consideration.itemType = ItemType.ERC20;
        consideration.token = address(wrappedrock); 
        consideration.identifierOrCriteria = 0;
        consideration.startAmount = 11;
        consideration.endAmount = 11;
        consideration.recipient = payable(address(ESCRW));

        ConsiderationItem memory consideration2;
        consideration2.itemType = ItemType.ERC721;
        consideration2.token = address(erc20s[0]); 
        consideration2.identifierOrCriteria = 100;
        consideration2.startAmount = 1;
        consideration2.endAmount = 1;
        consideration2.recipient = payable(address(bob.safe));

        OrderCreator.OrderToCreate memory orderToCreate2;
        orderToCreate2.offerer = bob;
        orderToCreate2.considerationItems = new ConsiderationItem[](2);
        orderToCreate2.considerationItems[0] = consideration;
        orderToCreate2.considerationItems[1] = consideration2;

        OrderMetadata memory payeeOrderMetadata;
        payeeOrderMetadata.orderType = OrderType.PAYEE;
        payeeOrderMetadata.rentDuration = 500;
        payeeOrderMetadata.emittedExtraData = new bytes(0);

        (Order memory payeeOrder, bytes32 payeeOrderHash) = _createSignedOrderTest(
            orderToCreate2.offerer,
            orderToCreate2.offerItems,
            orderToCreate2.considerationItems,
            payeeOrderMetadata
        );

        // create an order fulfillment for the pay order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payOrder,
            orderHash: payOrderHash,
            metadata: metadata
        });

        // create an order fulfillment for the payee order
        createOrderFulfillment({
            _fulfiller: bob,
            order: payeeOrder,
            orderHash: payeeOrderHash,
            metadata: payeeOrderMetadata
        });

        // add an amendment to include the seaport fulfillment structs
        withLinkedPayAndPayeeOrders({payOrderIndex: 0, payeeOrderIndex: 1});

        // finalize the order pay/payee order fulfillment
        (
            RentalOrder memory payRentalOrder,
            RentalOrder memory payeeRentalOrder
        ) = finalizePayOrderFulfillment();

        // get the rental order hashes
        bytes32 payRentalOrderHash = create.getRentalOrderHash(payRentalOrder);
        bytes32 payeeRentalOrderHash = create.getRentalOrderHash(payeeRentalOrder);

        // speed up in time past the rental expiration
        vm.warp(block.timestamp + 750);

        //Rental stop reverts on call, because ERC20s don't have the safeTransferFrom method implemented
        vm.expectRevert();
        stop.stopRent(payRentalOrder);
    }

    function _createSignedOrderTest(
        ProtocolAccount memory _offerer,
        OfferItem[] memory _offerItems,
        ConsiderationItem[] memory _considerationItems,
        OrderMetadata memory _metadata
    ) internal returns (Order memory order, bytes32 orderHash) {
        // Build the order components
        OrderComponents memory orderComponents = OrderComponentsLib
            .fromDefault(STANDARD_ORDER_COMPONENTS)
            .withOfferer(_offerer.addr)
            .withOffer(_offerItems)
            .withConsideration(_considerationItems)
            .withZoneHash(create.getOrderMetadataHash(_metadata))
            .withCounter(seaport.getCounter(_offerer.addr));

        // generate the order hash
        orderHash = seaport.getOrderHash(orderComponents);

        // generate the signature for the order components
        bytes memory signature = _signSeaportOrderRock(_offerer.privateKey, orderHash);

        // create the order, but dont provide a signature if its a PAYEE order.
        // Since PAYEE orders are fulfilled by the offerer of the order, they
        // dont need a signature.
        if (_metadata.orderType == OrderType.PAYEE) {
            order = OrderLib.empty().withParameters(orderComponents.toOrderParameters());
        } else {
            order = OrderLib
                .empty()
                .withParameters(orderComponents.toOrderParameters())
                .withSignature(signature);
        }
    }

    function _signSeaportOrderRock(
        uint256 signerPrivateKey,
        bytes32 orderHash
    ) private view returns (bytes memory signature) {
        // fetch domain separator from seaport
        (, bytes32 domainSeparator, ) = seaport.information();

        // sign the EIP-712 digest
        (uint8 v, bytes32 r, bytes32 s) = vm.sign(
            signerPrivateKey,
            domainSeparator.toTypedDataHash(orderHash)
        );

        // encode the signature
        signature = abi.encodePacked(r, s, v);
    }
}

```

Submitting this as a low because since the creator of the order is the owner of the NFT itself, its unlikely that they would be interested in this scenario playing out. In the future, if the protocol acquires interest in being able to create payee orders, this vulnerability could be used in order to steal the ERC20 provided.

### An user can frontrun `stopRentBatch()` in order to grief batch retrievals

In the scenario an user, or maybe the sponsors taking care of the protocol, wants to retrieve a large amount of orders, an user can frontrun it by calling `stopRent()`on one of the orders being submitted. This will cause the entire call stack to revert.

It's recommended that the loop function simply continues in case of a reversal instead.

### Kernel executor can steal all funds

`skim()` allows the owner to withdraw all funds that were accidentally sent to the payment escrow. The only addresses allowed to call this function are those who request permission from the kernel. The executor, being a power user of the kernel, can grant permission to one of their addresses to call `increaseDeposit()`, and increase the internal balancing of the contract. 

By increasing the internal balance of the contract by at least twice of what it is, the executor is then able to call `skim()` to withdraw all of the funds.

### Rental may last forever in case lender gets blacklisted

Certain tokens like USDC and USDT have a blacklist function which prevents these addresses from either receiving or sending tokens. If by any chance the lender of a base type order gets blacklisted, the token won't be transfered from the escrow, and `stopRent()` will always revert. The likelihood is extremely low as only few addresses were blacklisted, however its still possible.

# Non-Critical

### Update the rental contracts to gnosis safe v1.5.0 

A handful of key changes were done compared to the v1.4.1 version which may hinder usage of rental safes. 

For more information, visit https://github.com/safe-global/safe-contracts/blob/main/CHANGELOG.md