#### 1) Module.INIT is not implemented for both Storage and Escrow Modules.

#### 2) PaymentEscrow::MODULE_PROXY_INSTANTIATION can be front run by a bad actor

#### 3) Storage::MODULE_PROXY_INSTANTIATION can be front run by a bad actor

#### 4) All their contracts are using solidity version 0.8.20. They want to deploy to polygon and avalanche which only support up to solidity version 0.8.19 due to a breaking change from the PUSH0 opcode.

Reference: https://www.zaryabs.com/push0-opcode/
```☢️ Important Warning for Solidity Devs
Although PUSH0 opcode is now included with the latest solidity compiler, you need to be careful while using it on any other chain than ETH mainnet.

There are still other chains like L2s that don't recognize the PUSH0 opcode. Therefore, if you try to use the latest compiler to deploy your contract on any such chain that doesn't support this opcode, your contract deployment shall fail.```
