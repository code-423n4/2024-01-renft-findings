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

