
**Overview**

This protocol facilitates generalized collateral-free NFT rentals built on top of Gnosis Safe and Seaport.  The Default Framework is used as the main architecture for the protocol, and the contracts in scope can be categorized into four main groups: Modules, policies, packages and general.

**Scope**

> The engagement involved analysing twelve (12) different Solidity Smart Contracts:

- Payment Escrow.sol: Module dedicated to escrowing rental payments while rentals are active. When rentals are stopped, this module will determine payouts to all parties and a
fee will be reserved to be withdrawn later by a protocol admin.

- Storage.sol: Module dedicated to maintaining all the storage for the protocol. Includes
storage for active rentals, deployed rental safes, hooks, and whitelists.

- Admin.sol: Acts as an interface for all behavior in the protocol related to admin logic.
Admin duties include fee management, proxy management, and whitelist management.

- Create.sol: Acts as an interface for all behavior related to creating a rental. This is
the entrypoint for creating a rental through the protocol, which only Seaport contracts can access.

- Factory.sol: Acts as an interface for all behavior related to deploying rental safes.
Deploys rental safes using gnosis safe factory contracts.

- Guard.sol: Acts as an interface for all behavior related to guarding transactions that
originate from a rental wallet. Prevents transfers of ERC721 and ERC1155 tokens while a rental is active, as well as preventing token approvals and enabling of non-whitelisted gnosis safe modules.

- Stop.sol: Acts as an interface for all behavior related to stopping a rental. This policy
is also a module enabled on all rental safe wallets, and has the authority to pull funds out of a rental wallet if a rental is being stopped.

- Accumulator.sol: Package that implements functionality for managing dynamically allocated data struct arrays directly in memory. The rationale for this was the need for an array of structs where the total size is not known at instantiation.

- Reclaimer.sol: Retrieves rented assets from a wallet contract once a rental has been stopped, and transfers them to the proper recipient. A delegate call from the safe to
the reclaimer is made to pull the assets.

- Signer.sol: Contains logic related to signed payloads and signature verification when
creating rentals.

- Create2 Deployer.sol: Deployment contract that uses the init code and a salt to perform a deployment. There is added cross-chain safety as well because a particular
salt can only be used if the sender's address is contained within that salt. This prevents a contract on one chain from being deployed by a non-admin account on another chain.

- Kernel.sol: A registry contract that manages a set of policy and module contracts, as well as the permissions to interact with those contracts. Privileged admin and executor roles exist to allow for role-granting and execution of kernel functionality which includes adding new policies or upgrading modules.

# Codebase quality analysis

> Analysis of the codebase (What’s unique? What’s using existing patterns?):

**Unique:* 
- Codebase carries out specific governance mechanisms that are uniquely designed for reNFT’s specific use case e.g The recipient of ERC721 / ERC1155 tokens after rental creation is always the reNFT smart contract renter.

**Existing Patterns:* 
- The reNFT Protocol adheres to common contract management patterns, such as the use of OnlyRole and OnlyAdmin.

**Strengths:*
- Named imports of parent contracts are well utilised.
- Contracts use clear headers to identify different sections of code. 

**Weaknesses:*
- Lack of Official documentation
- Incomplete test coverage (80%). 100% test coverage is recomended  
- All contracts in scope use floating solidity pragma versions, this is not recomended.

# Mechanism review

- The mechanisms implemented in the reNFT ecosystem, including the ERC721 and ERC1155 standards are innovative and well-designed. They provide a comprehensive range of features and capabilities. However, there are some potential issues and risks associated with these mechanisms like the lack of documentaion on the mechanism to better understand it. These issues should be carefully considered and mitigated to ensure the security and reliability of the ecosystem.

# Centralization risks

Privileged functions can create points of failure: Ensure such accounts are protected and consider implementing multi sig to prevent a single point of failure. See [Here](https://github.com/code-423n4/2024-01-renft/blob/main/bot-report.md) for complete details.

# Systemic risks

- External Contract Dependencies: reNFT relies on external contracts from Openzepplin and Seaport just to name a few. If any of these contracts have vulnerabilities, it would affect the protocol.

- Test Coverage: The test coverage provided by reNFT is 80% however, 100% tests coverage is recommended.

- Like any smart contract-based system, reNFT is exposed to potential coding bugs or vulnerabilities. Exploiting these issues could result in the loss of funds or manipulation of the protocol.


# Documents

According to he Main audit page “there is no documentation outside of this readme and the actual code repo” It is highly recommended to create proper official documentation for the protocol. It would also be helpful for the Documentation to explaine how the ecosystem works from a basic contract level so that it is easier to digest for developers, users and auditors looking to integrate into the reNFT project.

I would also recommend adding quality Medium articles, it’s a great way to provide an indepth look at many of the topics in the project and is used by many blockchain projects.



# Architecture recommendations

> The reNFT architecture seems solid in general, none the less here are some areas that could be improved:

- Testing and Simulations: I recommend creating a live testnet app. Here is an
[example](https://app.opendollar.com/) from The Open Dollar protocol.
Conduct thorough testing of all contracts and functions and simulations to
understand how they will behave under various market conditions.

- I recommend rewriting some of the tests in the codebase for this audit to use the actual contracts instead of mock addresses like in some cases. This will offer greater confidence during system deployment.

**Gas Optimizations**

- Review data types: Analyze the data types used in your smart contracts and
consider if they can be further optimized. For example, changing uint256 to
uint128 or uint94 can save gas and storage slots.
- Struct packing: Look for opportunities to pack structs into fewer storage slots. By carefully selecting appropriate data types for struct members, you can reduce the overall storage usage.
- Use constant values: If certain values in your contracts are constant and do
not change, declare them as constants rather than storing them as state variables. This can significantly save gas costs.
- Avoid unnecessary storage: Examine your code and eliminate any unnecessary storage of variables or addresses that are not required for contract functionality.
- Storage vs. memory usage: When working with arrays or structs, consider whether using storage instead of memory can save gas. Using storage allows direct access to the state variables and avoids unnecessary copying of data.
- Replacing the use of memory with calldata for read-only arguments in external  functions.

**Other recommendations**

- Regular code reviews and adherence to best practices.
- Conduct external audits by security experts.
- Consider open sourcing the contract for community review.
- Maintain comprehensive security documentation.- Establish a responsible disclosure policy for vulnerabilities.
- Implement continuous monitoring for unusual activity.
- Educate users about risks and best practices.

# Approach taken in evaluating the codebase

> My analysis of reNFT Protocol Included understanding the architecture, mechanism, overall codebase and possible risks associated to the protocol. 

- Day 1: I spent time reading the different available articles in order to have a deep understanding of the protocol. 

- Day 2: I analyzed the codebase for better understanding, Performed a Mechanism review and investigated possible systemic risks, and centralization risks. 

- Day 3: I dedicated this day to coming up with possible Architecture recommendations after identifying possible risks and prepared the final analysis report.



## Conclusion
 
The reNFT ecosystem is a well-designed and innovative NFT platform, offering a wide range of features and capabilities. However, there are areas where improvements could be made, particularly in terms of permission management, gas efficiency, and potential exploitation of certain modules. By addressing these issues, the reNFT ecosystem could become even more secure, efficient, and reliable.



### Time spent:
24 hours