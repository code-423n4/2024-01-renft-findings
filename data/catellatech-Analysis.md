<div align="center">
  <h1> reNFT Protocol </h1>
  <h5>reNFT streamlines and secures the NFT rental process, providing a robust framework supported by smart contracts.</h5>
  <img src="https://global.discourse-cdn.com/standard17/uploads/renft/original/1X/aca6980bc4eb6cef344e05e5e30e58f119228e0e.png" width="200" height="200" >
</div>

## Index

- [Index](#index)
- [1. Introducing reNFT: A Comprehensive Overview](#1-introducing-renft-a-comprehensive-overview)
- [2. Diving into all the contracts provided by the protocol](#2-diving-into-all-the-contracts-provided-by-the-protocol)
  - [PaymentEscrow.sol](#paymentescrowsol)
  - [Storage.sol](#storagesol)
  - [Accumulator.sol](#accumulatorsol)
  - [Reclaimer.sol](#reclaimersol)
  - [Signer.sol](#signersol)
  - [Admin.sol](#adminsol)
  - [Create.sol](#createsol)
  - [Factory.sol](#factorysol)
  - [Guard.sol](#guardsol)
  - [Stop.sol](#stopsol)
  - [Create2Deployer.sol](#create2deployersol)
  - [Kernel.sol](#kernelsol)
  - [Privileged Roles](#privileged-roles)
- [3. Security Approach of the Project](#3-security-approach-of-the-project)
- [4. Addressing Architectural Risks](#4-addressing-architectural-risks)
  - [Architectural recommendations](#architectural-recommendations)
- [5. Systemic Risks](#5-systemic-risks)
- [6. Test analysis](#6-test-analysis)
  - [Implement your own stateful fuzzing test suite](#implement-your-own-stateful-fuzzing-test-suite)
  - [Building Your Invariants](#building-your-invariants)
  - [Write your Handler](#write-your-handler)
- [7. Recommendations beyond the code](#7-recommendations-beyond-the-code)

**The approach and steps we employed to examine the underlying code.**

Our approach to analyzing the source code of the **reNFT** Protocol was to simplify the information provided by the protocol, using a variety of diagrams to visually clarify the project's key contracts and break down each important part of these contracts. This enhances understanding for developers, security researchers, and users alike. We identified the fundamental concepts and employed simpler language to explain the functionality and goals of the **reNFT** Protocol. Furthermore, we organized the information logically into separate sections, each with identifying titles, to provide a clear overall picture of the subject. Our primary goal was to make the information more accessible and easy to understand.

> 🛑 Disclaimer: The provided diagrams are based on our interpretation of the protocol. In the event that any of them is not 100% accurate but aligns with the protocol's initial concept, we invite you to reach out to us via Discord (catellatech). We are willing to share the diagrams for the project team to make any necessary modifications. If the diagrams are deemed accurate, they are fully available for the protocol to use in its documentation.
> ✨ Make sure to click on "See Chart" to view the charts.

## 1. Introducing reNFT: A Comprehensive Overview

**reNFT** is a protocol that facilitates the rental of **NFTs** without the need for guarantees, built on platforms like **Gnosis Safe** and **Seaport**.
In this system, **NFT** owners, such as Alice, can express their willingness to lend their assets through **Seaport**. When a user, like Bob, wants to rent those NFTs, automated contracts are created. Bob receives a secure wallet **(Gnosis Safe)** with the rented NFTs, but cannot take them out of the wallet. The protocol employs various contracts **(Modules, Policies, and Packages)** to handle payments, storage, and rules.

**Overview diagram:**

<details>
<summary>See Chart</summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFTOverview.drawio.png?raw=true">
</details>

## 2. Diving into all the contracts provided by the protocol

### PaymentEscrow.sol

This contract is responsible for handling payments during and after rental contracts in the **reNFT protocol**, ensuring fair execution and reserving a fee for the protocol. It also provides functions to update the contract's implementation and collect accumulated fees.

<details>
<summary>See Chart</summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.PaymentEscrow.drawio.png?raw=true">
</details>

### Storage.sol

This contract is like the brain of the reNFT protocol. It handles stuff like adding and removing rentals, keeping track of safe contracts the protocol deploys, and setting up hooks for special actions in transactions.

It's got functions to tweak hook settings, handle addresses in whitelists, and even lets you update the contract to a fancier version if needed.

Think of it as the control center that makes sure everything runs smoothly in the world of digital asset rentals.

<details>
<summary>See Chart</summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Storage.drawio.png?raw=true">
</details>

### Accumulator.sol

This contract is a utility for managing dynamic arrays of rental asset updates (RentalAssetUpdate) in the project.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Accumulator.drawio.png?raw=true">
</details>

### Reclaimer.sol

- The Reclaimer contract acts as a mechanism for reclaiming rented assets after a rental has been stopped. It provides functions to transfer ERC721 and ERC1155 tokens.

- The contract ensures that the reclaimRentalOrder function is only callable through delegate calls by the rental safe (authorized by the Admin policy) and enforces that the reclaim can only be initiated by the specified rental wallet.

- The reclaim process involves looping through each item in the rental order, checking its type (ERC721 or ERC1155), and transferring it to the lender.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Reclaimer.drawio.png?raw=true">
</details>

### Signer.sol

This contract manages the verification and handling of signatures, ensuring that messages related to rental contracts maintain integrity and authenticity. Its purpose is to collaborate seamlessly with other contracts in the system, ensuring that only genuine and verified operations are carried out.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Signer.drawio.png?raw=true">
</details>

### Admin.sol

This contract handles important behind-the-scenes tasks in the system. It makes sure that only authorized admins can do important stuff and that everything needed by the system is set up the right way. It's kind of like the control center for managing fees, upgrades, and other crucial actions.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Admin.drawio.png?raw=true">
</details>

### Create.sol

This contract play a crucial role in reNFT system responsible for creating and verifying rental orders within a digital asset framework. The detailed functionality relies on how other contracts, like Zone, Policy, Signer, and others, are implemented and integrated into the system.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Create.drawio.png?raw=true">
</details>

### Factory.sol

This contract handling the deployment and management of rental safes in the reNFT environment. It connects with various modules and external contracts to enforce certain policies. The contract allows the initialization and deployment of rental safes with specific owners and signature requirements. It keeps track of deployed safes and emits events for monitoring. Factory contract plays a crucial role in the secure management of rental orders within the reNFT system.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Factory.drawio.png?raw=true">
</details>

### Guard.sol

This smart contract plays a crucial role in handling transactions from rental wallets. It sets up dependencies, asks for specific permissions, and safeguards against certain transactions, like preventing the transfer of rented tokens. It also collaborates with external hook contracts for extra processing during transactions. Admin functions let you update hook connections and control their status, with these powers limited to assigned admins. In a nutshell, the "Guard" contract boosts security by limiting transactions and adding extra features through external hooks in the realm of rental wallet activities.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Guard.drawio.png?raw=true">
</details>

### Stop.sol

This contract is for stopping rentals. It makes sure everything happens smoothly when you want to stop renting something. It checks if it's okay to stop based on the type of rental and other things. Then, it does some paperwork, like updating records and dealing with money. It also emits events to tell everyone that the rental is officially stopped.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Stop.drawio.png?raw=true">
</details>

### Create2Deployer.sol

This contract acts as a facilitator for deploying contracts using a special technique called **CREATE2**. It ensures a secure and verifiable deployment process, allowing only authorized individuals to deploy contracts to specific addresses.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Create2Deployer.drawio.png?raw=true">
</details>

### Kernel.sol

This contract serves as a registry overseeing policies and modules, facilitating their installation, upgrade, activation, and deactivation. Policies are activated with specific dependencies, and modules are installed or upgraded with permitted interactions governed by the **Kernel**.

<details> 
<summary> See chart </summary>
<img src="https://github.com/catellaTech/reNFT/blob/main/reNFT.Kernel.drawio.png?raw=true">
</details>

### Privileged Roles 
There are several important roles in the system.

* **SEAPORT**: Addresses assigned this role are authorized to interact with the **validateOrder()** function within the Create Policy. This role is intended for the Seaport core contract and should be granted exclusively to it.

* **CREATE_SIGNER**: Addresses holding this role are recognized as protocol signers with the authority to sign off on payloads initiating a rental.

* **ADMIN_ADMIN**: Addresses possessing this role are acknowledged as administrators of the Admin Policy, empowering them to perform administrative operations within the protocol.

* **GUARD_ADMIN**: Addresses endowed with this role have the capability to toggle whitelisted hook contracts and specify addresses deemed safe for a rental wallet to delegate calls.

> Given the level of control that these `roles` possess within the system, users must place full trust in the fact that the entities overseeing these `roles` will always act correctly and in the best interest of the `system` and its `users`.

## 3. Security Approach of the Project

- The security approach in the provided contracts is comprehensive, incorporating access control modifiers **(onlyKernel, onlyAdmin, and onlyRole)** to restrict functions to authorized entities.
- Error handling using revert ensures proper execution conditions. The upgradeable contracts pattern **(\_upgradeModule)** facilitates module updates without data loss.
- Access control and permissions, validated input data, and careful dependency management **(\_reconfigurePolicies)** enhance system integrity.
- Kernel migration **(\_migrateKernel)** is implemented cautiously.
- Event logging **(Events)** aids traceability.
- Consideration of gas limits, exception handling, adherence to security patterns, and Solidity version updates contribute to robust security.

## 4. Addressing Architectural Risks

**Throughout the audit process, specific architectural risks were pinpointed**:

- **Contract Upgrades**: Module upgrades (**\_upgradeModule**) can introduce risks if not handled carefully. Care must be taken when migrating data and ensuring that new modules are secure and compatible with previous versions.

- **Dependency Management**: Managing dependencies between **modules** and **policies** is critical. Changes in modules could affect **policies** that depend on them. The **\_reconfigurePolicies** function should handle dependency updates properly.

- **Permissions and Access**: Permission and access logic is crucial. Any errors in managing **roles**, **permissions**, and **functions** could compromise security. Thoroughly review and test the **onlyKernel**, **onlyAdmin**, and **onlyRole** functions.

- **Kernel Migration**: Kernel migration (**\_migrateKernel**) is a critical and risky process. Transitioning to a new kernel must be done carefully to avoid security issues and ensure a smooth transition of all functionalities.

- **Gas Limitations**: When **migrating** or **upgrading** contracts, gas limits should be considered. **Gas-expensive operations** may result in **failed transactions** or **denial-of-service** attacks.

### Architectural recommendations 
💡 A robust model is indispensable for systematically evaluating project risks. One highly recommended approach involves employing effective risk modeling techniques. https://www.notion.so/Smart-Contract-Risk-Assessment-3b067bc099ce4c31a35ef28b011b92ff#7770b3b385444779bf11e677f16e101e

💡 We suggest establishing a foundational architecture with a **System.sol** contract, serving as a central registry where all contracts are registered. The entire system can be organized around a singular contract, such as **SystemRegistry**. This contract acts as a cohesive entity that interconnects all other contracts, allowing for a comprehensive listing of all contracts within the system. Essentially, it functions as a registry, providing a centralized point to access information about all contracts in the system.. 

💡Evaluate the gas consumption of each function across different scenarios during testing to ensure optimal efficiency and identify potential areas for optimization.

## 5. Systemic Risks

**ERC1155 Token Standards**:

- The inclusion of the ERC1155 standard in the reNFFT protocol introduces certain risks, mainly stemming from its relative novelty and inherent complexity.

- Unexplored Vulnerabilities: Given its newer status compared to ERC20, ERC1155 hasn't undergone extensive real-world testing, especially in complex DeFi environments. This leaves room for undiscovered or unaddressed vulnerabilities.

- Complexity in DeFi Transactions: ERC1155's capacity to handle multiple asset types within a single contract adds intricacy. In a DeFi context, this complexity may lead to unforeseen challenges related to transaction handling, token tracking, and interoperability with other protocols or tokens.

**The Create2Deployer contract uses CREATE2 to deploy contracts**:

- This process involves generating contract addresses based on a salt and initialization code. It is crucial to ensure that the generation of these addresses is unique and not prone to collisions.

**Access Checks:**

- Some functions, such as **deploy** and **changeKernel**, have restrictions through modifiers like **onlyKernel** or **onlyAdmin**. It is essential to review and confirm that these access checks are sufficient to prevent unauthorized access.

**Kernel Updates**:

- The **changeKernel** function allows changing the address of the kernel contract. Caution should be exercised when performing this action as it can have significant implications for the security and operation of the system.

**Roles and Permissions Management:**

- Managing roles and permissions is critical for the proper functioning of the system. Thorough audits should be conducted to ensure that roles are granted and revoked correctly, and critical functions are adequately protected.

## 6. Test analysis 
> The audit scope of the contracts to be reviewed is 80%, with the aim of reaching 100%. 

However, The protocol provided a set of **Main invariants** that must always remain true. Using insights from the peculiarities of the **reNFT** system and its own testing suite, we realized that an integral part of test coverage was missing: **stateful fuzzing tests.**

### Implement your own stateful fuzzing test suite
- In your working environment, create a new folder called "**invariant**." In this folder, you will host two Solidity (.sol) files. The files will be named "**invariant.t.sol**" and "**handler.t.sol**," respectively. Once you have set this up, you will be ready to start writing your tests.

### Building Your Invariants
Start by writing "**invariant.t.sol**." You need to begin defining your tests by first building the 'invariant.'

### Write your Handler 
Now, regarding the creation of your **handler.t.sol** file. This contract, in particular, will be set up as an intermediary to restrict the contract that captures the state of the Fuz stateful.

Through the **handler**, you will instruct your **Foundry** and the **Stateful Fuzzing test suite** to correlate properly with the contract that captures the state of the **Fuz stateful**. Essentially, you are telling Foundry when to call the exact functions you want to test; all of this with precise guidelines to avoid explosive function calls.

With your tests set up, the next step is to write the "**handler**." Although you could write assertions directly in your invariants, the cleaner approach is to compute them in the "**handler**." This way, your assertions become a single line. This ensures that your logic is valid, regardless of variable input parameters. In the development of more complex software or systems, invariants play a crucial role in applying correctness.

## 7. Recommendations beyond the code
Attackers are becoming increasingly sophisticated, targeting protocols and their users not only through code vulnerabilities but also exploiting their social media presence, such as **X**. In these times more than ever, it is crucial to maintain robust layers of security. To enhance project security, it is imperative to establish control policies mandating **Two-Factor Authentication (2FA)** for all staff accounts and ensuring its widespread utilization whenever possible. It's important to acknowledge that achieving 100% foolproof security solely through smart contract codes is unattainable. To fortify security measures, consider implementing a more comprehensive policy that advocates for the use of **non-SMS 2FA methods**. For additional information, you can explore the recent **Simswap attack** on **Code4rena** detailed in this [Article](https://medium.com/code4rena/code4rena-twitter-x-incident-8b7f308a555d).

### Time spent:
28 hours