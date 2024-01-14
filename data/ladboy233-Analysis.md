# reNFT Audit Details Analysis Report

## Introduction

reNFT is a protocol that enables generalized collateral-free NFT rentals, utilizing Gnosis Safe and Seaport frameworks. This report analyzes the various aspects of the protocol, including its security features, module functions, policy mechanisms, and potential vulnerabilities based on the provided documentation.

The reNFT protocol presents an innovative approach to NFT rentals, utilizing existing frameworks like Gnosis Safe and Seaport. However, it faces challenges in ensuring the security of rental wallets and handling various token standards. The audit identifies key areas of concern and suggests potential improvements to enhance the protocol's robustness and reliability. As the protocol evolves, continuous monitoring and updating of security measures will be crucial in maintaining its integrity and user trust.

### Key Components

1. **Modules**: These are internal-facing contracts that store shared state across the protocol. Key modules include Payment Escrow and Storage.

2. **Policies**: External-facing contracts that handle inbound calls and route updates to data models via Modules. Notable policies include Admin, Create, Factory, Guard, and Stop.

3. **Packages**: Helper contracts like Accumulator, Reclaimer, and Signer assist in specific tasks within the protocol.

4. **General Contracts**: Contracts like Create2 Deployer and Kernel manage a variety of functionalities, including registry and policy management.

## Security Considerations

1. **Manipulation via Hook Contracts**: Hook contracts, serving as middleware, can potentially execute arbitrary logic, posing risks if malicious or faulty hooks are used.

2. **Dishonest or Malicious ERC721/ERC1155 Implementations**: The protocol can't prevent transfers via dishonest implementations that deviate from the standard ERC721/ERC1155 specifications.

3. **Rebasing or Fee-On-Transfer ERC20 Implementations**: The protocol doesn’t account for ERC20 tokens with fee-on-transfer or rebasing features, which can alter balances unexpectedly.

## Protocol Mechanics

1. **Rental Process**: The protocol facilitates NFT rentals where assets are transferred to a Gnosis safe created for the renter, with restrictions on asset transfer to ensure security.

2. **Payment Escrow**: Manages rental payments, ensuring proper distribution post-rental period.

3. **Admin Operations**: Include fee, proxy, and whitelist management, critical for maintaining the protocol’s integrity.

4. **Deployment and Registry Management**: Ensured by Create2 Deployer and Kernel contracts, respectively.

## Potential Attack Vectors

1. **Rental Wallet Security**: Ensuring rented assets can't be freely moved by the rental wallet is a primary concern.

2. **Rental Creation and Stopping**: Ensuring accurate logging and handling of rentals to prevent unauthorized asset movement or payment discrepancies.


### Centralization Risk and System Risk
- **Backend Server Owner's Signature:** A notable centralization risk arises from the requirement for the backend server owner to sign the `validateOrder` callback. This design choice introduces a potential single point of failure and undermines the decentralized nature of the system.
- **Dependency on Order Matching:** The system's reliance on the availability of a counterparty to fulfill orders presents a significant risk. In cases where no counterparty is interested, NFT lenders are unable to generate yield, leading to potential inefficiencies in the marketplace.

### Recommendations
Based on the analysis, the following high-level recommendations are made:
- **Enhance Decentralization:** Reconsider the design requiring the backend server owner’s signature to mitigate centralization risk.
- **Improve Order Matching Mechanisms:** Develop strategies to ensure efficient order matching and mitigate the risk of unfulfilled orders.
- **Secure Airdrop Claiming Process:** Implement measures to ensure that airdrops reach the intended recipient, the original lender.
- **Expand Payment Options:** Introduce native ETH token support to increase the protocol's adaptability and user convenience.
- **Strengthen NFT Transfer Security:** Address vulnerabilities specific to different NFT standards, such as ERC1155A, to prevent unauthorized transfers.
- **Handle Rebase Tokens Appropriately:** Modify the payment escrow mechanism to account for and manage rebase tokens effectively.

**Enhanced Security Measures for Hook Contracts**: Implementing robust validation and authorization mechanisms for hook contracts could mitigate potential exploits.

**Handling Non-Standard ERC721/ERC1155 Tokens**: Developing strategies to detect and manage non-standard implementations could enhance security.

**Accommodating Fee-On-Transfer and Rebasing ERC20 Tokens**: Adjusting the protocol to handle these types of ERC20 tokens can prevent balance discrepancies.

### Conclusion
The codebase exhibits several areas of concern, primarily related to centralization risks, system inefficiencies, and specific technical loopholes in NFT handling. Addressing these issues is crucial for the system's security, efficiency, and overall trustworthiness in the market.


### Time spent:
35 hours