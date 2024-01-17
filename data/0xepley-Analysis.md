# 🛠️ Analysis - ReNFT
***Collateral-free, permissionless, and highly customizable EVM NFT rentals.***

### Summary
| List |Head |Details|
|:--|:----------------|:------|
|a) |Overview of the reNFT Project| Summary of the whole Protocol |
|b) |The approach I would follow when reviewing the code | Stages in my code review and analysis |
|c) |Analysis of the code base | What is unique? How are the existing patterns used? "Solidity-metrics" was used  |
|d) |Test analysis | Test scope of the project and quality of tests |
|e) |Security Approach of the Project | Audit approach of the Project |
|f) |Other Audit Reports and Automated Findings | What are the previous Audit reports and their analysis |
|g) |Packages and Dependencies Analysis | Details about the project Packages |
|h) |New insights and learning of project from this audit | Things learned from the project |






## a) Overview of the reNFT Project

**reNFT** is a blockchain protocol designed for renting Non-Fungible Tokens (NFTs). It's structured to facilitate the renting process for NFT owners and renters, offering a secure and user-friendly platform.

### Key Features:

1. **Modular Architecture**: Separate contracts handle specific functions like payment escrow and storage.
2. **Integrations**: Incorporates Seaport for order processing and Gnosis Safe for wallet management.
3. **Security Focus**: Prioritizes the safety of rented assets and secure transaction handling.
4. **Efficient Rental Process**: Manages the rental lifecycle effectively.
5. **Access Control**: Implements role-based access for operational security.
6. **Token Support**: Compatible with ERC20, ERC721, and ERC1155 tokens.



## b) The approach I would follow when reviewing the code

First, by examining the scope of the code, I determined my code review and analysis strategy.
https://code4rena.com/audits/2024-01-renft

Accordingly, I would analyze and audit the subject in the following steps;

| Number |Stage |Details|Information|
|:--|:----------------|:------|:------|
|1|Compile and Run Test|[Installation](https://github.com/code-423n4/2024-01-renft?tab=readme-ov-file#tests)|Test and installation structure is simple, cleanly designed|
|2|Architecture Review| [ReNFT](https://github.com/re-nft/smart-contracts) |Provides a basic architectural teaching for General Architecture|
|3|Graphical Analysis  |Graphical Analysis with [Solidity-metrics](https://github.com/ConsenSys/solidity-metrics)|A visual view has been made to dominate the general structure of the codes of the project.|
|4|Slither Analysis  | [Slither Report](https://github.com/crytic/slither)| Slither report of the project for some basic analysis|
|5|Test Suits|[Tests](https://github.com/code-423n4/2024-01-renft?tab=readme-ov-file#tests)|In this section, the scope and content of the tests of the project are analyzed.|
|6|Manuel Code Review|[Scope](https://github.com/code-423n4/2024-01-renft?tab=readme-ov-file#scope)||
|7|Using Solodit for common vulnerabilities|[Solodit](https://solodit.xyz/)|Using solodit to find common vulnerabilites related to NFT protocol|
|8|Infographic|[Figma](https://www.figma.com/)|Tried to make Visual drawings to understand the hard-to-understand mechanisms|
|9|Special focus on Areas of  Concern|[Areas of Concern](https://github.com/code-423n4/2024-01-renft?tab=readme-ov-file#attack-ideas-where-to-look-for-bugs)|Code where I should focus more|

### Time spent:
5 hours