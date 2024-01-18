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
|f) |Codebase Quality | Overall Code Quality of the Project |
|g) |Other Audit Reports and Automated Findings | What are the previous Audit reports and their analysis |
|h) |Packages and Dependencies Analysis | Details about the project Packages |
|i) |New insights and learning of project from this audit | Things learned from the project |






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

## c) Analysis of the code base

The most important summary in analyzing the code base is the stacking of codes to be analyzed.
In this way, many predictions can be made, including the difficulty levels of the contracts, which one is more important for the auditor, the features they contain that are important for security (payable functions, uses assembly, etc.), the audit cost of the project, and the time to be allocated to the audit;
Uses Consensys Solidity Metrics

-  **Language:** language in which of the source is written
-  **nSLOC:** normalized source lines of code (only source-code lines; no comments, no blank lines)
-  **Comment Lines:** lines containing single or block comments
-  **Blank Lines:** Blank Lines in the source file
-  **Total Lines:** Total number of lines in the file 

## Analysis of sloc of modules contracts

[![Screenshot-from-2024-01-18-23-18-18.png](https://i.postimg.cc/t4RDL0Lp/Screenshot-from-2024-01-18-23-18-18.png)](https://postimg.cc/KRVnMWGH)

## Analysis of sloc of Pakages contracts

[![Screenshot-from-2024-01-18-23-15-40.png](https://i.postimg.cc/x87F96Ht/Screenshot-from-2024-01-18-23-15-40.png)](https://postimg.cc/4H615bW9)

## Analysis of sloc of Policies contracts

[![Screenshot-from-2024-01-18-23-17-12.png](https://i.postimg.cc/J09KfGVF/Screenshot-from-2024-01-18-23-17-12.png)](https://postimg.cc/CnsjDMKC)

## Comment-to-Source Ratio:

**Module contracts:** On average there are **0.8** code lines per comment (lower=better).

**Packages contracts:** On average there are **1.27** code lines per comment (lower=better).

**Policies contracts:** On average there are **1.35** code lines per comment (lower=better).

# Call Graph of Important Contracts

## Call graph of Kernel.sol

[![Screenshot-from-2024-01-17-21-47-36.png](https://i.postimg.cc/5NFrPQrw/Screenshot-from-2024-01-17-21-47-36.png)](https://postimg.cc/1fsMXXst)

## Call graph of Stop.sol

[![Screenshot-from-2024-01-17-21-50-10.png](https://i.postimg.cc/MT7LWCyd/Screenshot-from-2024-01-17-21-50-10.png)](https://postimg.cc/5YtggkKC)

## Call graph of Guard.sol

[![Screenshot-from-2024-01-17-21-51-25.png](https://i.postimg.cc/bdcdQ2Gz/Screenshot-from-2024-01-17-21-51-25.png)](https://postimg.cc/Fkp9vzgq)

## Call graph of Create.sol

[![Screenshot-from-2024-01-17-21-52-52.png](https://i.postimg.cc/CKmNmwsy/Screenshot-from-2024-01-17-21-52-52.png)](https://postimg.cc/xkNMdDZg)

# UML diagram of important Contracts
## UML diagram of PaymentEscrow.sol

[![Screenshot-from-2024-01-17-21-59-54.png](https://i.postimg.cc/c1bFp7tv/Screenshot-from-2024-01-17-21-59-54.png)](https://postimg.cc/5j85vC3J)


## UML diagram of Create.sol

[![Screenshot-from-2024-01-17-22-02-07.png](https://i.postimg.cc/66PNMv90/Screenshot-from-2024-01-17-22-02-07.png)](https://postimg.cc/QVcPtVK9)


## UML diagram of Signer.sol

[![Screenshot-from-2024-01-17-22-52-42.png](https://i.postimg.cc/02ZpwD78/Screenshot-from-2024-01-17-22-52-42.png)](https://postimg.cc/2VbbpqHJ)

## UML diagram of Guard.sol

[![Screenshot-from-2024-01-17-22-54-44.png](https://i.postimg.cc/SR1xySzp/Screenshot-from-2024-01-17-22-54-44.png)](https://postimg.cc/PvZjyk46) 

# Sequence diagram of the protocol
Sequence diagrams are a good way to understand the overall interaction of the classes or files in the protocol

[![Screenshot-from-2024-01-17-23-00-13.png](https://i.postimg.cc/K8CSmKqN/Screenshot-from-2024-01-17-23-00-13.png)](https://postimg.cc/pmQ1Zdth)

# Sequence diagrams of the important functions of the protocol

## `ValidateOrder()`
[![Screenshot-from-2024-01-17-23-04-45.png](https://i.postimg.cc/YSj6z0MJ/Screenshot-from-2024-01-17-23-04-45.png)](https://postimg.cc/xkDN00kR)

## `_settlePayment()`
[![Screenshot-from-2024-01-17-23-13-40.png](https://i.postimg.cc/kDr1k8X6/Screenshot-from-2024-01-17-23-13-40.png)](https://postimg.cc/q6LxKNK4)

## `stopRent()`
[![Screenshot-from-2024-01-17-23-20-39.png](https://i.postimg.cc/vBKx8tnB/Screenshot-from-2024-01-17-23-20-39.png)](https://postimg.cc/phDLq8qb)



## d) Test analysis
### What did the project do differently? ;
-   1) It can be said that the developers of the project did a quality job, there is a test structure consisting of tests with quality content. In particular, tests have been written successfully.

-   2) Overall line coverage percentage provided by your tests : 80

### What could they have done better?

-  1) In order to understand the test scenarios and develop more effective test scenarios, the following bob, alice and other roles are can be defined one by one, in this way role definitions increase the quality and readability in tests

```solidity

 // Sample labels
vm.label(bob, 'bob');
vm.label(alice, 'alice');
vm.label(DEPLOYER, 'deployer');
vm.label(USD_OWNER, 'usd owner');
vm.label(POOL_PROXY, 'lending pool');
```


-  2) If we look at the test scope and content of the project with a systematic checklist, we can see which parts are good and which areas have room for improvement As a result of my analysis, those marked in green are the ones that the project has fully achieved. The remaining areas are the development areas of the project in terms of testing ;


[![test-cases.jpg](https://i.postimg.cc/1zgD5wCt/test-cases.jpg)](https://postimg.cc/v1s40gdF)

Ref:https://xin-xia.github.io/publication/icse194.pdf

[![nabeel-1.jpg](https://i.postimg.cc/6qtBdLQW/nabeel-1.jpg)](https://postimg.cc/bDVXPnbW)

- 3) Test Coverage of the protocol is around 80% which is very less, the recommended test coverage of any protocol is above 90% so it is recommended to increase the coverage to at least 90%

# Test-case analysis of Contracts

### General Information
- **Solc Version:** 0.8.22
- **Block Limit:** 30,000,000 gas


### Storage.sol: Storage contract

Deployment Cost: 1,553,625
Deployment Size: 8,094

| Function Name             | Min    | Avg    | Median | Max    | # Calls |
|---------------------------|--------|--------|--------|--------|---------|
| INIT                      | 361    | 361    | 361    | 361    | 215     |
| KEYCODE                   | 229    | 229    | 229    | 229    | 430     |
| MODULE_PROXY_INSTANTIATION| 441    | 22,729 | 22,928 | 22,928 | 215     |
| addRentalSafe             | 487    | 28,733 | 24,787 | 44,687 | 1,069   |
| addRentals                | 1,046  | 49,008 | 51,497 | 53,997 | 40      |

### Admin.sol: Admin contract

Deployment Cost: 921,484
Deployment Size: 4,670

| Function Name             | Min    | Avg    | Median | Max    | # Calls |
|---------------------------|--------|--------|--------|--------|---------|
| changeKernel              | 795    | 795    | 795    | 795    | 2       |
| configureDependencies     | 4,104  | 47,496 | 47,904 | 47,904 | 215     |
| freezePaymentEscrow       | 8,093  | 33,399 | 46,052 | 46,052 | 3       |
| freezeStorage             | 8,017  | 33,335 | 45,994 | 45,994 | 3       |

### Create.sol: Create contract

Deployment Cost: 2,653,662
Deployment Size: 15,175

| Function Name             | Min    | Avg    | Median | Max    | # Calls |
|---------------------------|--------|--------|--------|--------|---------|
| changeKernel              | 750    | 750    | 750    | 750    | 2       |
| configureDependencies     | 4,104  | 47,496 | 47,904 | 47,904 | 215     |
| domainSeparator          | 280    | 280    | 280    | 280    | 36      |
| getOrderMetadataHash     | 2,272  | 2,678  | 2,272  | 4,277  | 36      |

### Factory.sol: Factory contract

Deployment Cost: 692,196
Deployment Size: 3,906

| Function Name             | Min    | Avg    | Median | Max    | # Calls |
|---------------------------|--------|--------|--------|--------|---------|
| changeKernel              | 773    | 773    | 773    | 773    | 2       |
| configureDependencies     | 2,369  | 24,065 | 24,269 | 24,269 | 215     |
| deployRentalSafe          | 647    | 311,013| 307,194| 334,994| 1,068   |
| initializeRentalSafe      | 50,851 | 50,853 | 50,851 | 53,351 | 1,066   |

### Guard.sol: Guard contract

Deployment Cost: 1,030,196
Deployment Size: 5,213

| Function Name             | Min    | Avg    | Median | Max    | # Calls |
|---------------------------|--------|--------|--------|--------|---------|
| changeKernel              | 750    | 750    | 750    | 750    | 2       |
| checkAfterExecution      | 329    | 329    | 329    | 329    | 7       |
| checkTransaction         | 1,844  | 16,044 | 14,463 | 29,012 | 35      |
| configureDependencies     | 2,346  | 23,941 | 24,246 | 24,246 | 216     |

### Stop.sol: Stop contract

Deployment Cost: 2,167,126
Deployment Size: 12,705

| Function Name             | Min    | Avg    | Median | Max    | # Calls |
|---------------------------|--------|--------|--------|--------|---------|
| changeKernel              | 728    | 728    | 728    | 728    | 2       |
| configureDependencies     | 4,059  | 47,051 | 47,859 | 47,859 | 217     |
| reclaimRentalOrder       | 8,532  | 23,159 | 28,432 | 30,932 | 13      |
| stopRent                  | 66,457 | 106,027| 100,096| 139,512| 7       |



## e) Security Approach of the Project

### Successful current security understanding of the project;

1- The project hasn't underwent any audits, this innovative assessments on Code4rena is the first, where multiple auditors are scrutinizing the code.

### What the project should add in the understanding of Security;

1- By distributing the project to testnets, ensuring that the audits are carried out in onchain audit. (This will increase coverage)

2- Add On-Chain Monitoring System; If On-Chain Monitoring systems such as Forta are added to the project, its security will increase.

For example ; This bot tracks any DEFI transactions in which wrapping, unwrapping, swapping, depositing, or withdrawals occur over a threshold amount. If transactions occur with unusually high token amounts, the bot sends out an alert. https://app.forta.network/bot/0x7f9afc392329ed5a473bcf304565adf9c2588ba4bc060f7d215519005b8303e3

3- After the Code4rena audit is completed and the project is live, I recommend the audit process to continue, projects like immunefi do this. 
https://immunefi.com/


4- Emergency Action Plan
In a high-level security approach, there should be a crisis handbook like the one below and the strategic members of the project should be trained on this subject and drills should be carried out. Naturally, this information and road plan will not be available to the public.
https://docs.google.com/document/u/0/d/1DaAiuGFkMEMMiIuvqhePL5aDFGHJ9Ya6D04rdaldqC0/mobilebasic#h.27dmpkyp2k1z

5- I also recommend that you have an "Economic Audit" for projects based on such complex mathematics and economic models. An example Economic Audit is provided in the link below;
Economic Audit with [Three Sigma](https://panoptic.xyz/blog/panoptic-three-sigma-partnership)

6 - As the project team, you can consider applying the multi-stage audit model.

[![sla.png](https://i.postimg.cc/nhR0kN3w/sla.png)](https://postimg.cc/Sn96Q1FW)

Read more about the MPA model;
https://mpa.solodit.xyz/

7 - I recommend having a masterplan applied to project team members (This information is not included in the documents).
All authorizations, including NPM passwords and authorizations, should be reserved only for current employees. I also recommend that a definitive security constitution project be found for employees to protect these passwords with rules such as 2FA. The LEDGER hack, which has made a big impact recently, is the best example in this regard;

https://twitter.com/Ledger/status/1735326240658100414?t=UAuzoir9uliXplerqP-Ing&s=19


## f) Codebase Quality

Overall, I consider the quality of the ReNFT protocol codebase to be Good. The code appears to be mature and well-developed. We have noticed the implementation of various standards adhere to appropriately. Details are explained below:

| Codebase Quality Categories              | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Code Maintainability and Reliability** | The ReNFT Protocol contracts demonstrates good maintainability through modular structure, consistent naming, and efficient use of libraries. It also prioritizes reliability by handling errors, conducting security checks. However, for enhanced reliability, consider professional security audits and documentation improvements. Regular updates are crucial in the evolving space.                                                                                                                                                                                                                                                                                                       |
| **Code Comments**                        | During the audit of the ReNFT contracts codebase, I found that some areas lacked sufficient internal documentation to enable independent comprehension. While comments provided high-level context for certain constructs, lower-level logic flows and variables were not fully explained within the code itself. Following best practices like NatSpec could strengthen self-documentation. As an auditor without additional context, it was challenging to analyze code sections without external reference docs. To further understand implementation intent for those complex parts, referencing supplemental documentation was necessary.                                                 |
| **Documentation** | The ReNFT project currently lacks comprehensive documentation, with only a basic readme file(code4rena audit page) available. We strongly recommend creating a detailed Whitepaper and documentation. These documents should provide an in-depth overview of the ReNFT protocol's architecture and functions, complemented by diagrams to illustrate contract interactions and functions. This documentation is essential to help users, developers, and auditors understand and trust the ReNFT's project.
| **Testing**                              | The audit scope of the contracts to be audited is modules, packages, Policies, Create2Deployer,  Kernel.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **Code Structure and Formatting**        | The codebase contracts are well-structured and formatted. but some order of functions does not follow the Solidity Style Guide According to the Solidity Style Guide, functions should be grouped according to their visibility and ordered: constructor, receive, fallback, external, public, internal, private. Within a grouping, place the view and pure functions last.                                                                                                                                                                                                                                                                                                                  |
| **Error**                                | Use custom errors, custom errors are available from solidity version 0.8.4. Custom errors are more easily processed in try-catch blocks, and are easier to re-use and maintain.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |


## g) Other Audit Reports and Automated Findings 

**Automated Findings:**
https://github.com/code-423n4/2024-01-renft/blob/main/bot-report.md

**Previous Audits**
There isn't any Previous Audit

**4naly3er report**
https://github.com/code-423n4/2024-01-renft/blob/main/4naly3er-report.md

##  h) Packages and Dependencies Analysis 📦

| Package | Version | Usage | 
| --- | --- | --- | 
| [`openzeppelin`](https://www.npmjs.com/package/@openzeppelin/contracts) | [![npm](https://img.shields.io/npm/v/@openzeppelin/contracts.svg)](https://www.npmjs.com/package/@openzeppelin/contracts) |  Project uses version `4.9.2`; consider updating to `5.0.1` 

## i) New insights and learning of project from this audit:

1. **Integration with Seaport**: The project's integration with Seaport for order processing demonstrates the complexity and potential challenges of interfacing with external protocols. This requires careful consideration of compatibility and security implications.

5. **Emergency Measures**: The project's code could benefit from implementing emergency measures like circuit breakers or pause functions, particularly in critical administrative functions, to mitigate risks in case of detected vulnerabilities.


Note: I didn't tracked the time, the time I mentioned is just an estimate






### Time spent:
3 hours