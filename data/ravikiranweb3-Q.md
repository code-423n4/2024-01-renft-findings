#### 1) Kernel::executeAction() sets Executor
The current executor can set the new executor via the executeAction() function in the Kernel. Executor role is a critical role and setting this to zero address or other address without access to private keys can be dangerous.

It is recommended to have a two step process for key roles in the protocol to minimise the risk of human error.

