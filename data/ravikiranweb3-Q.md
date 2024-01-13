#### 1) Kernel::executeAction() sets Executor user
The current executor can set the new executor via the executeAction() function in the Kernel. Executor role is a critical role and setting this to zero address or other address without access to private keys can be dangerous.

It is recommended to have a two step process for key roles in the protocol to minimise the risk of human error.

While this is reported as QA error, the risk of losing executor role completely is very high incase of a human error and once lost, there is no mechanism to reverse that mistake. This is not allowed for even admin.

Recommendation is to allow admin to set executor if required.

#### 2) Module.INIT is not implemented for both Storage and Escrow Modules.
