# QA Report

## Not all blockchain networks are compatible with Solidity version 0.8.20 and above

The Solidity 0.8.20+ compiler defaults to the [Shanghai EVM version](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which brings in the `PUSH0` opcode. However, not all Layer 2 solutions have adopted `PUSH0`, leading to potential deployment issues on those networks. To circumvent this, developers can opt for an earlier [EVM version](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) when compiling their contracts, as detailed in the [Foundry Book guidelines](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version).

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L2C1-L2C25

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L2C1-L2C25

```solidity
pragma solidity ^0.8.20;
```

When utilizing Solidity version 0.8.20 or higher for EVM smart contracts, it is advisable to be mindful of the EVM compiler version.

