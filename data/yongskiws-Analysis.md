#  Analysis - reNFT - Rent & Lend NFTs Protocol

The FIRST and ultimate NFT rental protocol for Web3 projects. reNFT is a multi-chain protocol and platform that can be white-labelled and integrated into any project to enable collateral-free in-house renting, lending, and scholarship automation!

# Overview 

Structuring the code is crucial for conducting a thorough analysis of the code base. This enables auditors to predict the level of contract difficulty, identify critical contracts, and determine security-critical features such as payment functions or assembly usage. Additionally, this approach accurately measures the time required to perform a detailed audit and helps determine the cost of a project audit.   To achieve efficiency, clarity, and improvements, it is essential to have a comprehensive view of the entire architecture. This enables the identification of the basic structure, relationships between modules, and key design patterns. By understanding the big picture, we can analyze the complexity of the code and reveal its strengths and potential for improvement with confidence.

-   **Lines:**  total lines of the source unit
-   **nLines:**  normalized lines of the source unit (e.g. normalizes functions spanning multiple lines)
-   **nSLOC:**  normalized source lines of code (only source-code lines; no comments, no blank lines)
-   **Comment Lines:**  lines containing single or block comments
-   **Complexity Score:**  a custom complexity score derived from code statements that are known to introduce code complexity (branches, loops, calls, external interfaces, ...)


| Type | File | Logic Contracts | Interfaces | Lines | nLines | nSLOC | Comment Lines | Complex. Score | Capabilities |
|------|------|-----------------|------------|-------|--------|-------|---------------|----------------|--------------|
| 📝 | contract\src\policies\Stop.sol | 1 | | 365 | 346 | 162 | 139 | 171 | ♻️ |
| 📝 | contract\src\policies\Guard.sol | 1 | | 379 | 339 | 161 | 144 | 135 | 🖥♻️ |
| 📝 | contract\src\policies\Factory.sol | 1 | | 194 | 180 | 78 | 82 | 74 | 🧮 |
| 📝 | contract\src\policies\Create.sol | 1 | | 776 | 719 | 365 | 273 | 245 | ♻️ |
| 🎨 | contract\src\packages\Signer.sol | 1 | | 409 | 371 | 195 | 136 | 117 | 🧮 |
| 🎨 | contract\src\packages\Reclaimer.sol | 1 | | 102 | 102 | 41 | 49 | 26 | |
| 🎨 | contract\src\packages\Accumulator.sol | 1 | | 146 | 140 | 46 | 72 | 133 | 🖥 |
| 📝 | contract\src\modules\Storage.sol | 2 | | 373 | 349 | 106 | 193 | 117 | |
| 📝 | contract\src\modules\PaymentEscrow.sol | 2 | | 433 | 402 | 154 | 202 | 96 | |
| 📝🎨 | contract\src\Kernel.sol | 4 | | 617 | 592 | 222 | 289 | 185 | |
| 📝 | contract\src\Create2Deployer.sol | 1 | | 121 | 112 | 44 | 56 | 56 | 🖥💰🧮🌀 |
| 📝🎨 | Totals | 16 | | 3915 | 3652 | 1574 | 1635 | 1355 | 🖥💰🧮🌀♻️ |


### Diagrams Risk 1.1
![Risk](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/ac2bd83b-5d8e-47ef-8708-fbcd7721d5d6)

### Diagrams Source Lines (sloc vs. nsloc) 1.2
![Source lines](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/2926c50a-35c0-4e8c-8cc3-1986175d166b)

### Components 1.3 
| Contracts | Libraries | Interfaces | Abstract |
|-----------|-----------|------------|----------|
|     10    |     0     |      0     |     6    |


### Exposed Functions 1.4 
| Public | Payable |
|--------|---------|
|   63   |    1    |

| External | Internal | Private | Pure | View |
|----------|----------|---------|------|------|
|    56    |   122    |    7    |  19  |  34  |

### StateVariables 1.5
| Total | Public |
|-------|--------|
|   51  |   35   |

### Capabilities 1.6
| Solidity Versions observed | 🧪 Experimental Features | 💰 Can Receive Funds | 🖥 Uses Assembly | 💣 Has Destroyable Contracts |
|---------------------------|------------------------|--------------------|---------------|---------------------------|
|         ^0.8.20           |           -            |        yes         |      yes (7 asm blocks)      |            -              |

| ♻️ TryCatch | Σ Unchecked |
|----------|-------------|
|   yes    |      -      |

### Dependencies / External Imports 1.7

| Dependency / Import Path                                        | Count |
|------------------------------------------------------------------|-------|
| @openzeppelin-contracts/token/ERC1155/IERC1155.sol               |   1   |
| @openzeppelin-contracts/token/ERC20/IERC20.sol                   |   1   |
| @openzeppelin-contracts/token/ERC721/IERC721.sol                 |   1   |
| @openzeppelin-contracts/utils/cryptography/ECDSA.sol            |   1   |
| @safe-contracts/SafeL2.sol                                      |   1   |
| @safe-contracts/base/GuardManager.sol                            |   1   |
| @safe-contracts/common/Enum.sol                                  |   2   |
| @safe-contracts/handler/TokenCallbackHandler.sol                |   1   |
| @safe-contracts/proxies/SafeProxyFactory.sol                    |   1   |
| @seaport-core/lib/rental/ConsiderationStructs.sol              |   1   |
| @seaport-types/lib/ConsiderationStructs.sol                     |   1   |
| @solady/utils/LibString.sol                                     |   3   |
| @src/Kernel.sol                                                 |   6   |
| @src/interfaces/IHook.sol                                       |   3   |
| @src/interfaces/ISafe.sol                                       |   3   |
| @src/interfaces/IZone.sol                                       |   1   |
| @src/libraries/Errors.sol                                       |  10   |
| @src/libraries/Events.sol                                       |   4   |
| @src/libraries/KernelUtils.sol                                  |   5   |
| @src/libraries/RentalConstants.sol                              |   1   |
| @src/libraries/RentalStructs.sol                                |   8   |
| @src/libraries/RentalUtils.sol                                  |   4   |
| @src/modules/PaymentEscrow.sol                                  |   2   |
| @src/modules/Storage.sol                                        |   4   |
| @src/packages/Accumulator.sol                                   |   2   |
| @src/packages/Reclaimer.sol                                     |   1   |
| @src/packages/Signer.sol                                        |   2   |
| @src/packages/Zone.sol                                          |   1   |
| @src/policies/Guard.sol                                         |   1   |
| @src/policies/Stop.sol                                          |   1   |
| @src/proxy/Proxiable.sol                                        |   2   |
| src/libraries/Events.sol                                         |   1   |


### AST Node Statistics (FUNCTIONS CALLS) 1.8.1
![AST](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/4b455b22-6e2f-49fa-a595-63b0354ff334)

### Assembly Calls 1.8.2
![Asembly](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/0669728b-876e-4e01-b746-bc1bbc73f8f5)

### AST Total 1.8.3
![AST TOTAL](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/d2588023-e9c0-41a0-ba62-5ea6e586a7b8)

### AST Total 1.8.3

# Codebase quality

### Strengths 2.1 :

Exemplary unit test coverage: The codebase shines with its meticulous unit testing strategy. The comprehensive suite of unit tests ensures the reliability and resilience of the system's core functionality, providing confidence in its performance under various scenarios.

Artful integration of Natspec: A commendable strength is the thoughtful integration of Natspec. The use of this documentation format not only improves code readability, but also exemplifies a commitment to providing clear and human-friendly documentation that fosters a more collaborative and accessible development environment.

### Weaknesses 2.2 :

Opportunities to improve documentation: While the codebase stands out in many ways, there is an opportunity for refinement in the documentation. Addressing this area of improvement can increase the overall clarity of the codebase, make it more accessible, and facilitate seamless collaboration among developers.
In essence, the codebase demonstrates proficiency in testing methodologies and documentation practices. Strengthening the documentation further would strengthen the codebase's comprehensibility and contribute to an even more polished and developer-friendly ecosystem.

# Approach we followed when reviewing the code 3

- PaymentEscrow.sol The contract is designed to manage payments during the lease term by providing an escrow to store payments. Key features include fee handling, proportional or full payment settlement, and proxy upgrade capabilities. Contracts are designed for integration into larger rental systems.

- Storage.sol The Storage module confidently manages storage for a protocol. It includes active lease-related storage, protocol-enforced safe storage, hooks, and whitelists. This contract is designed to be upgradable and provides display functions and external functions to interact with and modify storage data.

- Create.sol This module interacts seamlessly with other modules in the protocol and streamlines the creation of rental orders by efficiently processing valid Seaport orders.

- Stop.sol This contract is the definitive way to terminate a lease and implements a Kernel Policy that is linked to the Storage and PaymentEscrow modules. It contains internal functions for validation, hook processing, and recovery of leased assets. 

- Kernel.sol registry expertly manages modules, policies, and permissions for seamless interaction. This contract confidently enables the installation, upgrading, and activation of modules and policies. Several trusted roles, including SEAPORT, CREATE_SIGNER, ADMIN_ADMIN, and GUARD_ADMIN, possess special permissions.


# Analysis of the code base and diagram architecture 4

# Create.Sol
![Create](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/23c4aa47-d05f-4465-96da-554005517da2)

![Karnels](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/c11e83d9-a9f1-421d-ad1d-d1894c7c724d)

Our functions perform specific tasks with precision and expertise.
We process goods and manage
orders, verify the safety of rental
owners, validate execution, and ensure that assets belong to the correct owner.
Manage rental orders and handle rental-related inquiries over the phone with confidence.

The 'validateOrder' function is
a callback function that Seaport calls after processing each order in the batch.
It validates signatures, recovers owners, and initializes the payload from Seaport.
Validate order metadata, process items, and update status updates in Storage.
Update deposits in PaymentEscrow and call hooks regarding rental.
Emit the RentalOrderStarted event to notify that the rental order has started.

The view functions confidently retrieve important information, such as domain separators and hashes, that are directly related to rental orders.

This workflow clearly outlines the contract's interaction with the Seaport, including validation, data processing, and the production of status updates and events.


  
# Kernel.Sol
![Karnel Adapter Graph](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/45123efc-1b56-4ae8-9699-002131fa3b37)

![Karnelsss](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/3daeceda-1a9a-412c-9ba9-1200f14e4a07)

## Internal Functions:

### functions of the module installation:
``Explanation: Used to manage the installation of new modules in the kernel.  
Discussion: Ensure that module installation is tied to internal kernel management and cannot be called from outside the contract.```

### function of upgrade module:
Explanation: Responsible for the module upgrade process in the kernel.
Discussion: Ensure that module upgrades can only be triggered internally, thereby maintaining full control over the module.

### Policy Activation Functions:
Description: Handles the policy activation process in the kernel.
Discussion: Provides internal control over policy activation and ensures that policies are only activated by authorized entities.

### Policy Deactivation Functions:
Description: Handles the policy deactivation process in the kernel.
Discussion: Manage policy deactivation securely and ensure that policies can only be deactivated by authorized
entities.

### Kernel Migration Functions:
Description: Handles the kernel migration process from the old kernel contract to the new kernel contract.
Discussion: Provides full control over the kernel migration process and ensures that this action is performed only internally.

### policy reconfiguration functions:
Explanation: Performs reconfiguration of policies that depend on a module after the module is upgraded.

Discussion: Ensures that dependent policies can adapt to new module addresses after an upgrade.

### function of granting and revoking policy permissions:
Description: Manages the process of granting or revoking policy permissions to interact with modules.
Discussion: Provides internal controls over policy permissions and ensures that permissions are granted only by authorized entities.

### Restrict internal functions of dependencies:
Explanation: Removes the policy from the dependency array of the affected module.
Discussion: Clean up dependencies that are no longer needed after the policy is disabled.

## External Function:

### Perform External Function Action:
Description: Performs various actions such as install/module, upgrade/module, enable/policy, disable/policy, kernel migration, and executor/admin changes.
Discussion: Provides an external interface that can be invoked to interact with the kernel and control various aspects of the system.

### External Role Assignment Function:
Explanation: Assigns a role to a given address.
Discussion: Provides an external way to manage access rights and roles in the system.

### External Role Revocation Functions:
Explanation: Revokes the role from the specified address.
Discussion: Provides an external way to manage access rights and roles in the system.

# Stop.sol
![stop](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/77ee90f4-0612-4329-9a71-1d9c9c396418)
![stopppppz](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/9a4faf5b-a282-4c19-bdbb-726dbc906095)

## Internal functions:

- _emitRentalOrderStopped
Description: Dispatch an event indicating that a rental order has been stopped.
Discussion: Provides information to parties monitoring the blockchain that a rental order has been stopped. a

#### - _validateRentalCanBeStoped
Description: Validates whether a rental order can be stopped based on the order type and expiration time.
Discussion: Ensure that only rental orders that meet certain criteria can be stopped, such as only base orders can be stopped after the expiration time.

#### -_reclaimRentedItems
Description: Reclaim leased items from the lessee back to the lessor.
Discussion: Handling the return of rented items from the renter to the lender after the rental order has ended.

#### - _removeHooks
Description: Handle any hooks associated with terminated rentals.
Discussion: Process any hooks associated with terminated rental requests to perform specific functions after termination.

## External Functions:

#### -stopRent
Description: Stops a single rental order by validating, processing, reclaiming the asset, completing payment, removing from storage, and sending the event.
Discussion: Provides a mechanism for terminating a rental order and includes necessary operations such as payment and asset management.

#### -stopRentBatch
Description: Terminates a group of rent orders by validating, processing, reclaiming assets, completing payments, removing from storage, and broadcasting events for each rent order.
Discussion: Provides the ability to stop a number of rental orders at once by performing the same operations as the stopRent function.


# PaymentEscrow.sol
![paymentescrow](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/4942790d-7b3c-42a8-bb3f-58b325883337)

## Internal functions:

### Calculate fee:
_calculateFee(uint256 amount): Calculates the fee amount based on a given percentage of the payment.
Discussion: Determine the amount of fees charged on payments.

### Secure token transfer:
_safeTransfer(address token, address to, uint256 value): Facilitates the secure transfer of ERC20 tokens, handling various success and failure scenarios.
Discussion: Ensures secure transfer of ERC20 tokens, handling various success and failure scenarios.

### Proportional payment calculation:
_calculatePaymentProRata(uint256 amount, uint256 elapsedTime, uint256 totalTime): Calculates the proportional payment amount for the tenant and lender based on the elapsed time.
Discussion: Calculate the proportional payment amounts for the tenant and lender based on the elapsed time.

### Proportional Payment Settlement:
_settlePaymentProRata(...): Settles payments proportionally, distributing the amount to the lender and the tenant.
Discussion: Settle payments proportionally, distributing the amount to lenders and tenants.

### Settle payment in full:
_settlePaymentInFull(...): Settles the payment in full, transferring the entire amount to the lender or lessee.
Discussion: Settles the payment in full, sending the entire amount to the lender or lessee.

## External functions:

### Settle payment for an order:
settlePayment(RentalOrder calldata order): Settles the payment for a rental order using internal settlement logic.
Discussion: Called externally to settle payment for a rental order.

### Settle payments for multiple orders:
settlePaymentBatch(RentalOrder[] calldata orders): Settles payments for multiple rental orders in one call.
Discussion: Called externally to settle payments for multiple rental orders at once.

### Deposit tokens:
increaseDeposit(address token, uint256 amount): Increase the tracked token balance when a user deposits tokens.
Discussion: Called externally when a user deposits a token into escrow.

### Cost configuration:
setFee(uint256 feeNumerator): Sets the escrow fee percentage.
Discussion: Called externally to configure the fee percentage applied during payment settlement.

### Token Skimming and Fee Collection:
skim(address token, address to): Collects protocol fees and manages funds inadvertently sent to escrow.
Discussion: Called externally to manage and retrieve additional funds sent to the escrow contract.

### Contract upgrades:
Upgrade (address new implementation): Allows the contract to be upgraded externally to a different implementation, following the proxiable pattern.
Discussion: Invoked externally to perform contract upgrades.

### Contract freezing:
freeze(): Freezes the contract, preventing further upgrades.
Discussion: Called externally to lock the contract and prevent further upgrades.


# Storge.sol

![storage](https://github.com/yongsxyz/Audit-Analysis/assets/39027215/eb4a2f7c-f55f-4ec9-8fc8-43a5520af9df)

## View Functions:

### isRentedOut:
Description: Checks if an asset is rented by a specific wallet.
Discussion: Used to determine if an asset is actively leased in the log, assisting with asset management and rental status.

### ContractToHook:
Description: Retrieves the hook address associated with the destination address.
Discussion: It is important to determine which hook will act as an intermediary when there is a transaction from the rental wallet to a specific destination address.

### hookOnTransaction, hookOnStart, hookOnStop:
Description: Determines whether certain functions are enabled on the hook.
Discussion: Used to manage and monitor the activation status of specific functions on specific hooks during the transaction process.

## External Functions:

### addRentals:
Description: Adds rental order hashes and updates rental asset information in memory.
Discussion: Records and updates the rental status of assets and marks rental orders as active.

### removeRentals:
Description: Removes rental order hashes and updates rental asset information in the repository.
Discussion: Used to end the rental of an asset and delete rental orders that are no longer active.

### RemoveRentalsBatch:
Description: Batch version of removeRentals, removes multiple rentals at once.
Discussion: Makes it easy to remove multiple rental orders at once, helping to manage rental orders more efficiently.

### addRentalSafe:
Description: Add a rental wallet address to the store.
Discussion: Records rental safe addresses issued by the log, making it easier to identify and manage issued safes.

### updateHookPath:
Description: Updates the hook associated with a target address.
Discussion: Used to bind hooks as middleware for transactions from rental wallets to specific destination addresses, allowing customization of hook functionality during the transaction process.

### updateHookStatus:
Description: Update the hook status with a bitmap showing its functionality.
Discussion: Setting the activation status of certain functions on a hook provides flexibility in determining the hook's response to transactions.

### toggleWhitelistDelegate:
Description: Enables or disables delegate calling capability for an address.
Discussion: Provides control over delegate call capabilities for specific addresses, customizing access levels and security.

### ToggleWhitelistExtension:
Description: Enables or disables a permitted extension.
Discussion: Allows or disallows the use of Gnosis secure modules in rental wallets, providing control over which modules can be used.

### Upgrades:
Description: Upgrade the contract to another implementation.
Discussion: Allows upgrades to newer contract versions, preserving the protocol's ability to evolve over time.

### Freeze:
Description: Freezes contracts, preventing further contract upgrades.
Discussion: Putting a contract in a static state is useful when you want to ensure contract stability and prevent unexpected changes.






### Time spent:
16 hours