# [L01]: The RentalOrder hash does not match the RentalOrder struct.

When hashing RentalOrder, the following is done
```solidity
    function _deriveRentalOrderHash(
        RentalOrder memory order
    ) internal view returns (bytes32) {
        ...
        return
            keccak256(
                abi.encode(
                    _RENTAL_ORDER_TYPEHASH,
                    order.seaportOrderHash,
                    keccak256(abi.encodePacked(itemHashes)),
                    keccak256(abi.encodePacked(hookHashes)),
                    order.orderType,
                    order.lender,
                    order.renter,
                    order.startTimestamp,
                    order.endTimestamp
                )
            );
```
However, this is missing `order.rentalWallet` present in the `RentalOrder` struct

[RentalStructs.sol#L133C1-L143C2](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/libraries/RentalStructs.sol#L133C1-L143C2)
```solidity
struct RentalOrder {
    bytes32 seaportOrderHash;
    Item[] items;
    Hook[] hooks;
    OrderType orderType;
    address lender;
    address renter;
    address rentalWallet;
    uint256 startTimestamp;
    uint256 endTimestamp;
}
```

Therefore a malicious renter can modify the `rentalWallet` field of the original `RentalOrder` struct with and call `stopRent` as the hash computed will be the same as the `rentalWallet` is not taken into account while hashing.

On the current solidity versions deployed by the team, this is not exploitable because it causes a arithmetic underflow due when executing `removeRentals`
[Storage.sol#L216C1-L219C6](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L216C1-L219C6)
```
    function removeRentals(
        bytes32 orderHash,
        RentalAssetUpdate[] calldata rentalAssetUpdates
    )
    ...
        for (uint256 i = 0; i < rentalAssetUpdates.length; ++i) {
            RentalAssetUpdate memory asset = rentalAssetUpdates[i];

            // Reduce the amount of tokens for the particular rental ID.
            rentedAssets[asset.rentalId] -= asset.amount;
        }
```
Since the `asset.rentalId` is generated using the address of the `rentalWallet`, `rentedAssets[asset.rentalId]` is 0 initially, and when decrementing by `asset.amount` will cause arithmetic underflow and revert.

However, in the offchance the team decides to use versions less than solidity 8.0 in the future, then the consequence is disastrous as the function will not revert and the order hash will be deleted from storage even though the malicious renter supplied the wrong `rentalWallet`, after which the lender can no longer reclaim the rental NFT due to the missing order hash.

Attached is a PoC which demonstrates how a revert occurs if a malicious renter modifies the `rentalWallet` of the rental order, due to arithmetic underflow:

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.20;

import {Order, OfferItem, ItemType} from "@seaport-types/lib/ConsiderationStructs.sol";
import {Enum} from "@safe-contracts/common/Enum.sol";

import {
    OrderType,
    OrderMetadata,
    RentalOrder
} from "@src/libraries/RentalStructs.sol";
import {Errors} from "@src/libraries/Errors.sol";

import {BaseTest} from "@test/BaseTest.sol";

import {ISafe} from "@src/interfaces/ISafe.sol";

contract FakeSafe is ISafe { 
    function execTransaction(
        address to,
        uint256 value,
        bytes calldata data,
        Enum.Operation operation,
        uint256 safeTxGas,
        uint256 baseGas,
        uint256 gasPrice,
        address gasToken,
        address payable refundReceiver,
        bytes memory signatures
    ) external payable returns (bool success) { return true; }

    function execTransactionFromModule(
        address to,
        uint256 value,
        bytes calldata data,
        Enum.Operation operation
    ) external returns (bool success) { return true; }
    
    function swapOwner(address prevOwner, address oldOwner, address newOwner) external {}
    function enableModule(address module) external {}
    function disableModule(address prevModule, address module) external {}
    function setGuard(address guard) external {}
    function getTransactionHash(
        address to,
        uint256 value,
        bytes calldata data,
        Enum.Operation operation,
        uint256 safeTxGas,
        uint256 baseGas,
        uint256 gasPrice,
        address gasToken,
        address refundReceiver,
        uint256 _nonce
    ) external view returns (bytes32 transactionHash) { return bytes32(""); }
    function setup(
        address[] calldata _owners,
        uint256 _threshold,
        address to,
        bytes calldata data,
        address fallbackHandler,
        address paymentToken,
        uint256 payment,
        address payable paymentReceiver
    ) external {}
    function isOwner(address owner) external view returns (bool) { return true; }
    function nonce() external view returns (uint256) { return 0; }
}

contract FakeSafe_Test is BaseTest {
    // hook contract
    function test_FakeSafe() public {
        // create a BASE order where a lender offers more ERC1155 in
        // exchange for some erc20 tokens
        createOrder({
            offerer: alice,
            orderType: OrderType.BASE,
            erc721Offers: 1, 
            erc1155Offers: 0,
            erc20Offers: 0,
            erc721Considerations: 0, 
            erc1155Considerations: 0,
            erc20Considerations: 1
        });

        // finalize the order creation
        (  
            Order memory order,
            bytes32 orderHash,
            OrderMetadata memory metadata
        ) = finalizeOrder();

        // create an order fulfillment
        createOrderFulfillment({
            _fulfiller: bob,
            order: order,
            orderHash: orderHash,
            metadata: metadata
        });

        // finalize the base order fulfillment. This executes the token swap
        // and starts the rental.
        RentalOrder memory rentalOrder = finalizeBaseOrderFulfillment();

        // ======== EXPLOIT ========
        // speed up in time after the rental period
        vm.warp(block.timestamp + 750);

        // stop the rental order
        vm.prank(bob.addr);
        rentalOrder.rentalWallet = address(new FakeSafe());
        stop.stopRent(rentalOrder);
    }
}
```

Result:
```
    │   ├─ [7970] STORE::removeRentals(0x234ffb294231b7028aeb4e9327eecd567e6aadce024adc984adfaa5ffeddc2ab, [RentalAssetUpdate({ rentalId: 0x5ae01b1080d6d1715c135a6b9d0f2a36b5150e579a40e3a5833e29f2a1e84ac7, amount: 1 })])
    │   │   ├─ [7625] STORE_IMPLEMENTATION::removeRentals(0x234ffb294231b7028aeb4e9327eecd567e6aadce024adc984adfaa5ffeddc2ab, [RentalAssetUpdate({ rentalId: 0x5ae01b1080d6d1715c135a6b9d0f2a36b5150e579a40e3a5833e29f2a1e84ac7, amount: 1 })]) [delegatecall]
    │   │   │   ├─ [2917] kernel::modulePermissions(0x53544f5245000000000000000000000000000000000000000000000000000000, StopPolicy: [0x21CA97969FCeD24DFb0aA660179aDcF5E0ff3da2], 0x3fd9b3ee00000000000000000000000000000000000000000000000000000000) [staticcall]
    │   │   │   │   └─ ← true
    │   │   │   └─ ← panic: arithmetic underflow or overflow (0x11)
    │   │   └─ ← panic: arithmetic underflow or overflow (0x11)
    │   └─ ← panic: arithmetic underflow or overflow (0x11)
    └─ ← Error != expected error:  != panic: arithmetic underflow or overflow (0x11)
```

# [L02]: Using solidity 8.20 and above may be incompatible with some chains

The project uses solidity 8.20 and above to compile their contracts. solidity 8.0 by default uses Shanghai EVM to compile the contracts which may include `PUSH0` opcode. The project looks to expand to all the chains that are supported by Safe. However, it may be incompatible with some chains supported by Safe such as Arbitrum, due to the `PUSH0` opcode not being supported if the default EVM version is used.

Reference: [https://docs.arbitrum.io/for-devs/concepts/differences-between-arbitrum-ethereum/solidity-support#:~:text=address%20aliasing.-,OPCODE%20PUSH0,-This%20OPCODE%20is](https://docs.arbitrum.io/for-devs/concepts/differences-between-arbitrum-ethereum/solidity-support#:~:text=address%20aliasing.-,OPCODE%20PUSH0,-This%20OPCODE%20is)

