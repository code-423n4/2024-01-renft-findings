# QA Report

## Not all blockchain networks are compatible with Solidity version 0.8.20 and above

The Solidity 0.8.20+ compiler defaults to the [Shanghai EVM version](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which brings in the `PUSH0` opcode. However, not all Layer 2 solutions have adopted `PUSH0`, leading to potential deployment issues on those networks. To circumvent this, developers can opt for an earlier [EVM version](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) when compiling their contracts, as detailed in the [Foundry Book guidelines](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version).

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L2C1-L2C25

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L2C1-L2C25

```solidity
pragma solidity ^0.8.20;
```

When utilizing Solidity version 0.8.20 or higher for EVM smart contracts, it is advisable to be mindful of the EVM compiler version.

## Unnecessary `payable` Modifier
The `deploy()` function in the `Create2Deployer` contract is marked as `payable`, which means that the function is allowed to receive Ether along with the call. 

Here's the relevant part of the code:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L32

```solidity
function deploy(
    bytes32 salt,
    bytes memory initCode
) external payable returns (address deploymentAddress) {
    // Function body...
}
```

The `payable` modifier on the `deploy()` function may not be necessary. This could be an issue if there is no intention for the `deploy()` function to handle Ether or if the contract does not require Ether to be sent to it for any other reason. In this context, the `payable` modifier could be a potential vulnerability because it allows anyone to send Ether to the contract without a clear mechanism to withdraw it, possibly leading to a situation where Ether is locked in the contract forever.

If the `deploy()` function is not meant to forward Ether to the contract, the `payable` modifier should be removed to prevent the accidental sending of Ether to the `Create2Deployer` contract.

Here is how the function signature would look without the `payable` modifier:

```solidity
function deploy(
    bytes32 salt,
    bytes memory initCode
) external returns (address deploymentAddress) {
    // Function body...
}
```

Removing the `payable` modifier ensures that no Ether can be sent to the `deploy()` function. If Ether is sent to a non-payable function, the transaction will be rejected, and the Ether will remain with the sender. This helps prevent the accidental loss of funds and ensures that the contract behavior is clear and explicit.

## Inconsistency in Event Emission for `Stop` Contract

The `Stop` contract defines two functions, `stopRent` and `stopRentBatch`, for stopping rental orders individually or in batches. However, there's a discrepancy in how the `Events.RentalOrderStopped` event is emitted in these two functions regarding the order hash data emitted.

* Single Order Stop Function (`stopRent`):

When stopping a single rental order, the `stopRent` function emits the `RentalOrderStopped` event using the `seaportOrderHash` directly from the `order` parameter:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L305

```solidity
function stopRent(RentalOrder calldata order) external {
    // ... (omitted for brevity)
    _emitRentalOrderStopped(order.seaportOrderHash, msg.sender);
}
```

* Batch Order Stop Function (`stopRentBatch`):

In contrast, when stopping a batch of rental orders, the `stopRentBatch` function emits the `RentalOrderStopped` event using a hash derived from the order data, `_deriveRentalOrderHash(orders[i])`, for each order in the batch:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L356C37-L356C48

```solidity
function stopRentBatch(RentalOrder[] calldata orders) external {
    // ... (omitted for brevity)
    for (uint256 i = 0; i < orders.length; ++i) {
        // ... (omitted for brevity)
        orderHashes[i] = _deriveRentalOrderHash(orders[i]);
        // ... (omitted for brevity)
        _emitRentalOrderStopped(orderHashes[i], msg.sender);
    }
}
```

The inconsistency lies in the source of the order hash used in the event emission:

- In `stopRent`, the `seaportOrderHash` is used, which suggests that it is the original hash of the order as it was created or recognized by the system.
- In `stopRentBatch`, `_deriveRentalOrderHash(orders[i])` computes a new hash for each order based on its contents, rather than using an existing `seaportOrderHash`.

This discrepancy could lead to potential confusion or inconsistencies in tracking and handling events off-chain. If an external system or service relies on these events for processing or analytics, the difference in emitted values might cause issues such as:

- Difficulty in correlating events from single and batch operations as pertaining to the same underlying rental order.
- Data integrity concerns if the computed hash does not match the original order hash in all cases.
- Inconsistencies in event logs, making it challenging to create a reliable history or audit trail of rental order stops.

The contract should standardize the data emitted in events to ensure consistency. If `seaportOrderHash` is the canonical identifier for orders, it should be used in both single and batch operations.