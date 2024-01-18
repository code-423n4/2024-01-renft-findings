**Index of table**
| Sl.no  | Particulars  |
|---|---|
| 1   | Overview  |
| 2  | Architecture view(Diagram)  |
| 3  | Approach taken in evaluating the codebase  |
| 4  | Systematic risks  |
| 5 | Mechanism review  |
| 6  | Recommendation   |
| 7  | Hours spend  |


## 1. Overview

Rental NFT is a protocol that introduces an innovative, emerging concept. It allows NFT owners to list their assets for rent, specifying rental terms and conditions such as duration, pricing, and any usage restrictions. Renters can then browse the available NFTs and select those they wish to rent. Upon agreement to the terms, they pay the rental fee, and the NFTs are securely transferred to their wallets, granting them temporary control and usage rights. Throughout the rental period, owners earn passive income as rental payments are directly deposited into their wallets. Upon completion of the rental period, the NFTs are automatically returned to their original owners, ensuring a seamless and secure process for both parties.


## 2. Architecture view(Diagram) 

![DIAgram](https://user-images.githubusercontent.com/69415766/297624752-71d0c338-ff46-4059-b657-c8948fdbc37a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDU1NTUxMDcsIm5iZiI6MTcwNTU1NDgwNywicGF0aCI6Ii82OTQxNTc2Ni8yOTc2MjQ3NTItNzFkMGMzMzgtZmY0Ni00MDU5LWI2NTctYzg5NDhmZGJjMzdhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMTglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTE4VDA1MTMyN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTczNDQ1YjFhOWQ3M2Q3NjBjZjIyYTIxM2RhMTA5ZTBmNjY5NmZlZjBlYjEwMjExYWUwNTljN2Q1ZGYzNzU0OGEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.zrghZMJGi6X_ynjWxbvUhrXsIi_XZqLJq5EQMEFSQNo)



## 3. Approach taken in evaluating the codebase

We approached manual code review by looking into each space in the scoped contract and decided to explain some core functional contracts.It is important to note that manual code reviews can be very time-consuming.

**Core Contracts**

1. Modules
    * PaymentEscrow.sol
    
    * Storage.sol


 **PaymentEscrow.sol**
 
This contract plays a vital role in facilitating payments and settlements between renters and lenders. It holds the logic for fee collection, pro-rata payment mechanisms, payment-in-full settlements with specified order types, and a skim mechanism for protocol fees. The skim mechanism enables the recovery of funds that are accidentally sent to the payment escrow contract.

 * Core functions
  
  `_calculateFee()`
  
This function calculates the fee by considering the fee variable, expressed in basis points, and the amount of tokens involved.

    
   `_calculatePaymentProRata()`
    
This function distributes allocated tokens proportionally based on elapsed and total time stamps. It takes three arguments: amount, elapsed, and totalTime. It first calculates the numerator by multiplying the amount and elapsed  time. Then, it determines the renter's amount by dividing the numerator by totalTime. Finally, it subtracts the renter's amount from the original amount to obtain the lender's amount.


   `_settlePayment()`

This function facilitates payments through both pro-rata and full payment methods based on the specified order type. It accepts an array of items, the order type (an enum), lender and renter addresses, and rental start and end timestamps. It first calculates the elapsed time and total rental duration. It then iterates through each item, identifying ERC20 token payments, calculating any applicable fees, and deducting them from the payment amount. The token balance is then adjusted accordingly.The function determines the payment settlement approach based on the order type and rental status if pay orders where the rental is ongoing, it calls the `_settlePaymentProRata()` function to proportionally distribute the payment between the lender and renter or pay orders that have ended or for BASE orders, it calls the `_settlePaymentInFull()` function to execute a full settlement between the lender and renter.


  `skim()`
  
This function is used to send the fees amount that was accidentally sent to the paymentEscrow contract. It takes two arguments: token and to address. First, it fetches the balance of the token and the balance of the token in this contract. It then subtracts these amounts to get the skimmed amount and transfers it to the destination address.


 **Storage.sol**

This contract plays a vital role by storing essential data related to rentals, deployed safes, hooks, and whitelisted addresses for delegate calls by safes. It provides critical functionalities such as adding and removing rentals, adding rental safes, and updating hooks. These capabilities ensure the smooth and secure management of rental agreements and assets within the reNFT protocol.

  * Core functions 
  
  `addRentals()`

This function is usde to addition of rental hashes to storage by accepting two arguments `orderHash` and `rental asset`. It commences by setting the `orderHash` to true, indicating its presence in storage. Subsequently, it leverages a for-loop to update the order hash for the corresponding item with the specified amount, ensuring accurate and consistent storage of rental data.


  `removeRentals()`
  
This function act as the inverse of the `addRentals()` function. It commences by verifying the validity of the provided orderHash argument. Subsequently, it leverages a for-loop to systematically reduce the amount specified within the `rentalAssetUpdates` dynamic array arguments, ensuring accurate inventory management and data consistency.


  `addRentalSafe()`

This function serves to register new safes within the storage system, effectively increasing the total number of safes available for use within the protocol.


  `updateHookPath()`

This function is used to update the hook path, which is essential for proper transaction handling when interacting with guards. If items have associated hooks, the transaction is seamlessly directed to the hook address, which is diligently maintained and updated by this function. This ensures that necessary actions or checks are performed by the hook before transaction completion, enhancing security and integrity within the protocol.


  `updateHookStatus()`
  
This function serves to update the hook status with a valid bitmap that falls below the threshold of 7 (0111). It commences by validating that the provided bitmap adheres to this constraint. Upon successful validation, it proceeds to diligently update the specified hook address.


2.Policies

  * Create.sol
  
  * Factory.sol
  
  * Guard.sol
   
  * Stop .sol
  
  
**Create.sol**

This contract plays a vital role in creating the rental safe, validating rental orders received from the Seaport zone parameters, adding hooks for rental orders, and processing different order types according to their specifications.

 * Core functions 


  `validateOrder()`
  
This function acts as a entry point for initiating rental processes within the reNFT protocol, designed to interact seamlessly with Seaport. It's callback by Seaport after handling each order in a batch when the create policy is specified as the zone address, effectively kickstarting the rental process. This function decodes the extra data needed from the signed rental zone payload, constructing a payload of pertinent Seaport data, verifying the validity of the protocol signer's signature and the fulfiller's identity, recovering the signer's address for authorization checks, ensuring the signer possesses the `CREATE_SIGNER` role, and, upon successful validation, decisively triggering the rental process via the `_rentFromZone()` function. It concludes by returning `validOrderMagicValue` selector.


  `_executionInvariantChecks()`
  
This function plays a critical role in verifying asset ownership and distribution after a Seaport order is executed. It validates each execution, ensuring that ERC20 tokens are transferred to the designated payment escrow module,confirming that rental assets are securely deposited into the intended recipient's rental safe, and reverting transactions for unsupported item types.


  `_rentFromZone()`
  
This internal function validates rental payloads and Seaport payloads to initiate rental orders. It performs multiple validation and invariant checks on the provided arguments, calls the `_convertToItems()` function to verify and process Seaport order and consideration items, and populates a dynamic array of items. It then conditionally handles base and pay orders through a for-loop, identifying rental items and inserting them into the dynamic array. Subsequently, it constructs a rental order struct, calls `addRentals()` to store it in global storage, increases token deposits in the payment escrow, and optionally calls `_addHooks()` to execute associated hooks if specified.


  `_addHooks()`
  
This function is used to add the hooks to offer through the for-loop it checks whether the target address is approved by the reNFT and assign offer hook to specified index and calls `Hook.onStart()` function when a rental has been started.

  
  `_convertToItems()`
  
This function is used to converts Seaport order data into a format compatible with the reNFT protocol by first creating an array of Item structs. It then systematically processes offer and consideration items based on specific order types: for base orders, it uses specified functions to handle both offer and consideration items for pay orders, it processes offer items and enforces zero consideration items for payee orders, it enforces zero offer items and processes consideration items and for unsupported order types, it reverts with an error.


  `_processPayeeOrderConsideration()`
  
This function processes the consideration for payee orders. It uses a for-loop to check whether the consideration is in ERC20 or rental products only. It then increases the total payments and rentals, ensuring that the payee order includes at least one rental and one payment.


  `_processBaseOrderConsideration()`
  
This function processes the consideration of base orders for ERC20 tokens only. It utilizes a for-loop to verify that each consideration item is indeed an ERC20 token. If so, it populates the Item dynamic array with the specified variables.

  
  `_processPayOrderOffer()`
  
This function processes pay orders by taking three arguments an Item dynamic array, offers, and a startIndex. It begins with a for-loop containing if-else statements to determine the offer type (ERC721, ERC1155, or ERC20). It then increments the total rentals and total payments accordingly, populating the `Item` dynamic array with specified variables.

  `_processBaseOrderOffer()`
  
This function processes base orders by accepting three arguments dynamic arrays named `Item` and `SpentItems`, along with a `startIndex`. It initiates with a for-loop that utilizes if-else statements to ascertain the offer type (either ERC721 or ERC1155). Finally it populates the Items dynamic array with designated variables.


**Factory.sol**

This contract plays a crucial role by deploying new rental safes, initializing them, requesting necessary permissions, and configuring dependencies.

  * Core functions
  
  `initializeRentalSafe`
  
This function initializes the rental safe by adding a stop policy to halt rental process activities and a guard policy to scrutinize transactions initiated by the rental safes.

  `deployRentalSafe()`
  
This function creates and initializes a new rental safe with specified owners and a transaction threshold. It first validates the threshold to ensure it's within an acceptable range. Next, it prepares data for a delegated call to enable the rental manager module within the safe. It then constructs initialization data payload for the safe, including owners, threshold, and additional configurations. The function proceeds to deploy the safe proxy using a unique salt nonce. Finally, it calls `addRentalSafe()` function to stores the deployed safe's address, emits a deployment event, and returns the safe's address to the caller.


**Guard.sol**

This contract serves as an intermediary between the rental safe and the target destination. Its primary responsibility is to check the transactions for the absence of hooks within rental items. If a hook is detected, the transaction is seamlessly transferred to the hook address for further handling.

 * Core functions 
  
  `checkTransaction()`
  
This is external function serves as a gatekeeper for transactions originating from rental safes. It first prohibits delegate calls unless explicitly whitelisted. It then enforces the presence of a function selector within the transaction data. Next, it retrieves any associated hook for the target contract and verifies its activation status. If a hook is present and active, the function redirects control flow to the hook for further handling. In the absence of a `hook` address, the function performs a primary transaction check using an internal `_checkTransaction` fucntion.


  `_forwardToHook()`
  
This private function is invoked when a hook address is associated with a rental safe. It accepts five arguments the `hook` address, `safe` address, `recipient` address, `transaction` value, and `transaction` data. It then directly calls the `onTransaction()` function of the designated hook contract, enabling the hook to intervene in transactions involving rented assets and active hooks.

  `_checkTransaction()`
  
This private function is called when a hook address is lacking within the rental safe. It uses Yul inline assembly to extract the function selector invoked within the transaction. It achieves this by loading the initial 20 bytes of the data variable and storing the extracted selector within the selector variable. It then employs a series of if-else statements to meticulously examine whether the transaction calls upon an authorized function, as determined by the selector variable. The following transactions are deemed permissible:

  *  `e721_safe_transfer_from_1_selector`
  
  *  `e721_safe_transfer_from_2_selector`
    
  *  `e721_transfer_from_selector`
  
  *  `e721_approve_selector`
  
  *  `e1155_safe_transfer_from_selector`

  *  `gnosis_safe_enable_module_selector`

  *  `gnosis_safe_disable_module_selector`
  
The above function selector is allowed on transaction with specified extension.
  
    
  *  `shared_set_approval_for_all_selector`
      
  *  `e1155_safe_batch_transfer_from_selector`
  
  *  `gnosis_safe_set_guard_selector`

The above selector are not allowed on transaction.

**Stop.sol**

This contract plays a crucial role in deactivating or halting the rental activities of a rental safe under various circumstances. These triggers can include, but are not limited to, an owner reclaiming their NFT ownership from the renter. This contract will associated with rental safe while intialising the safe.

 * Core functions
 
 `stopRent()` 

This function act as a interface between rental owner and rented assets to stop the rental activites and get back his NFT by settlement process. First it calls `_validateRentalCanBeStoped()` function to commences with a validation check to ensure the rental can be lawfully halted. It then constructs a data structure to track rental asset updates. It uses for-loop to iterates through each item within the order, appending rental asset updates for rented items. If hooks are associated with the rental, it diligently removes them by calling the `_removeHooks()` function. It proceeds to reclaim rented assets, transferring them back to the lender's possession. It then settles any pending ERC20 payments via the escrow contract by calling `settlePayment()`. Finally, it calls `removeRentals()` function to remove rentals from storage.

  
  `_validateRentalCanBeStoped()`
  
This function is used to validate whether a transaction is eligible for stopping by verifying the order type and expected lender.


  `_removeHooks()`
  
This function is used to remove hooks that are specified in rental safes. First, it employs a for-loop to iterate through the given arguments to ensure that the rented item can be removed. It then assigns the item to the indexed rental items and finally calls the `onStop()` function within `Hook.sol` to manage the stop transaction of rented items.



3. Packages 
 
 * Accumulator.sol
 
 * Reclaimer.sol
 
 * Signer.sol
 
 
**Accumulator.sol**

This is an abstract contract that has helper functions for adding and removing rental safe lists to a dynamic list and for determining array data types from bytes data types.

  * Core functions
  
  
 `_insert()`

This is internal function which is used to called while adding new rental assets to a dynamically managed bytes array in Solidity. It extracts the rental ID, leverages assembly code for memory operations, and initializes the array if empty. It calculates size adjustments by adding some bytes of data (0x20 , 0x40) , determines memory locations for the new ID and amount, and carefully updates the free memory pointer to ensure memory safety.


 `_convertToStatic()`

This function is used to converts a dynamically managed bytes array of rental asset updates into a fixed-size array of `RentalAssetUpdate` struct. It directly extracts the array length using assembly code, allocates a new struct array, extracts rental IDs and amounts using precise memory calculations, and intialise new structs to assign the fixed-size array.


**Reclaimer.sol**

This is an abstract contract that has helper functions for reclaiming the assets after rental stopped and transfers them to proper recipient.


 `reclaimRentalOrder()`
 
This function is specifically designed to facilitate the secure return of rented assets to their rightful owner or recipient. It have a multi-validation system to ensure authorized reclaims. The function can only be invoked through a delegated call, and exclusively by the designated rental safe. It verifies the request's origin from the specified rental wallet before proceeding with the transfer of assets back to the lender. To ensure compatibility with various asset types, it uses the specialized functions `_transferERC721()` for ERC721 tokens and `_transferERC1155()` for ERC1155 tokens.


**Signer.sol**

This contract plays a vital role in the reNFT protocol by verifying and validating transactions, deriving hashes from structured data types, and deriving hashes from static data types.



## 4. Systematic risks

After manual code review of reNFT protocol by looking each space we are concluded by following risks.
Systemic risk is the possibility that a single event could cause an industry or the economy to collapse.


 * Market risk
 
The value of NFTs can fluctuate significantly. This means that there is a risk that the borrower will not be able to repay the loan, or that the value of the NFT will fall below the value of the collateral.


 * Smart contract risk
 
The rental process is carried out through a smart contract, which must undergo multiple audits, including unit tests and formal verification. These extensive audits help to ensure a thorough understanding of the codebase.

   - Front-run risk  :-
   
Malicious actors could front-run the transaction for the function below and manipulate the owner array argument, posing a security risk to both users and the associated protocol and there is no authorisation validation.


   
   ```solidity
       function deployRentalSafe(
        address[] calldata owners,
        uint256 threshold
    ) external returns (address safe) {
        // Require that the threshold is valid.
        if (threshold == 0 || threshold > owners.length) { // check the statements again Analysis.
            revert Errors.FactoryPolicy_InvalidSafeThreshold(threshold, owners.length);
        }

        // Delegate call from the safe so that the rental manager module can be enabled
        // right after the safe is deployed.
        bytes memory data = abi.encodeCall(
            Factory.initializeRentalSafe,
            (address(stopPolicy), address(guardPolicy))
        );

        // Create gnosis initializer payload.
        bytes memory initializerPayload = abi.encodeCall(
            ISafe.setup,
            (
                // owners array.
                owners,
                // number of signatures needed to execute transactions.
                threshold,
                // Address to direct the payload to.
                address(this),
                // Encoded call to execute.
                data,
                // Fallback manager address.
                address(fallbackHandler),
                // Payment token.
                address(0),
                // Payment amount.
                0,
                // Payment receiver
                payable(address(0))
            )
        );
   
   ```


 * Trusted role risk 
 
The executor role carries inherent risk that only the existing executor can modify the executor address within the Kernel smart contract, which serves as an intermediary between policies and modules. It implements and manages a set of policies and modules, adds or revokes roles for specified target addresses, and even migrates the Kernel address itself. These crucial responsibilities, along with other delegated roles, concentrate significant power within the executor position. Consequently, if the executor account is compromised, all associated policies and modules become vulnerable.




## 5. Mechanism review 

Rental infrastructure is a new ground breaking innovative concept which makes lender can earn through renting by this protocol.


 * Increased liquidity for NFT owners 
 
The reNFT protocol allows NFT owners to earn income from their NFTs by renting them out. This can increase the liquidity of NFTs and make them more attractive to investors.
 
 
 * Access to NFTs for borrowers 
 
The reNFT protocol makes it possible for borrowers to access and use NFTs that they would not otherwise be able to afford. This can benefit both individual users and businesses.
 

 * Security and transparency 
 
 The reNFT protocol uses smart contracts to automate the rental process and ensure that both parties are protected. All transactions are recorded on the blockchain, which provides a high degree of transparency.
 
 
 * Rental Process
 
Rental Process create vast opporunities for both lenders and borrowers by entering an economic activities, the lender can creates and signs a Seaport order, which will be validated by the reNFT protocol. A new Gnosis Safe will be created to hold the lender's NFT, and hooks will be added to handle the transaction. After basic validation operations, the NFT will be added to the rented list with the agreed-upon terms and payment. The owner can halt the rental process at any time and call for settlement. The person who takes temporary ownership of the NFT can utilize it for any sector, such as in a game that requires an NFT to play, by paying the agreed-upon amount.

 
 * Increased liquidity for NFT owners:
 
The reNFT protocol allows NFT owners to earn income from their NFTs by renting them out. This can increase the liquidity of NFTs and make them more attractive to investors.
  
  
 * Access to NFTs for borrowers
 
The reNFT protocol makes it possible for borrowers to access and use NFTs that they would not otherwise be able to afford. This can benefit both individual users and businesses.


 * Rental period

During the rental period, the borrower can use the NFT as they see fit, as long as they comply with the rental terms.


 * Settlement

At the end of the rental period, the NFT is automatically transferred back to the owner. The borrower's collateral is released from the escrow smart contract. 



**Comparison table of competietive protocol**


| Feature  | OpenSea  | reNFT  |
|---|---|---|
| Primary Focus	  | Buying and selling NFTs  | NFT rental marketplace  |
| Token Standards  | ERC-721, ERC-1155, ERC-20  | ERC-721, ERC-1155, ERC-20 and specific standards optimized for rentals |
| Rental Features  | Limited (via third-party integrations)  | Core functionality, native rental contracts  |
| Security Deposits  | Typically handled by third-party integrations  | Built-in escrow system for security deposits  |
| Fees  | 2.5% transaction fee for sellers  | Varies based on rental terms and platform fees  |
| Dispute Resolution  | Limited, often relies on external parties  | Built-in arbitration system  |
| User Experience  | Mainstream, user-friendly interface  | Emerging, focusing on rental-specific features  |
| Scalability  | Large-scale, supports multiple blockchains  | Large-scale, supports multiple blockchains  |
| Governance  | Centralized  | Decentralized, community-driven  |
| Rental Terms  | Varies based on integration  | Customizable rental periods, payment schedules, usage rules  |
| User Base     | Large, established community  | Growing community, focused on NFT utility                    |
| Ownership Model | Transfer of full ownership	| Temporary access through rental agreements |
| Asset Types   | Broad range of NFTs (art, collectibles, gaming, etc.) | Emphasis on utility-based NFTs (e.g., in-game items, tools) | 
| Rental Features | Limited rental support for select NFTs | Comprehensive rental framework with customizable terms and agreements |


## 6. Recommendation 

After manual observation of code and mechanism we recommend a simple changes which make more robustness to reNFT protocol.

1. We recommend to add the setter function where trusted admin role can set the new executor role address if any chance executor address got compromised

Add the below function in Kernal.sol

```solidity

function setExecutor( address new_Executor) onlyAdmin {
   
   executor = new_Executor;

}

```
The above recommendation effectively secures the kernel, its related policies, and modules in the face of security risks.


2. Implement a TimeLock mechainsm in reNFT protocol for crucial action are:-

 * Kernal.sol

The kernel contract plays a vital role in managing policies, modules, and migrating the kernel address. The reNFT protocol can implement a time-lock mechanism for these operations, introducing a delay period to decide whether changes are necessary. This enables protocol team members to determine if changes pose any risks before implementation.


 * Factory.sol
 
This contract is used to deploy new rental safes with proxy features. Implementing a time-lock mechanism would enable trusted roles or protocol members to cancel executions if the NFT assets themselves pose any risks. This would help build the integrity of the protocol.


3. We notice that the reNFT protocol plans to support ERC-20 tokens for payments and settlements beyond fee-on-transfer and rebasing tokens. However, if an accounting mechanism is implemented in the future, we recommend considering a solution for handling ERC-20 tokens with different decimals.


4. We recommend to add trusted role modifier to deploy the safe of rental contract in order to prevent the front-run transaction risk.

```solidity

   function deployRentalSafe(
        address[] calldata owners,
        uint256 threshold
    ) external onlyRole("TrustedRole") returns (address safe) { //@audit change here
        // Require that the threshold is valid.
        if (threshold == 0 || threshold > owners.length) { // check the statements again Analysis.
            revert Errors.FactoryPolicy_InvalidSafeThreshold(threshold, owners.length);
        }

        // Delegate call from the safe so that the rental manager module can be enabled
        // right after the safe is deployed.
        bytes memory data = abi.encodeCall(
            Factory.initializeRentalSafe,
            (address(stopPolicy), address(guardPolicy))
        );

```

The above code snippet prevents the fron-run risk and it could implemented in reNFT.


5. We observed that reNFT will be deployed on multiple chains, with governance restricted to trusted roles and protocol members. We recommend adding a community proposal system where potential lenders or borrowers can propose changes through a voting mechanism using specified tokens. This would enable empowered community members to contribute to the protocol's growth with these tokens, creating economic opportunities for both the protocol and community. Additionally, it would attract diverse users from across multiple chains.


6. We notice that reNFT protocol would like begin Ethereum Mainnet, Polygon and Avalanche and like to expand to all the chains that are supported by safe . However arbitrum chain also supports to safe we recommend to take measure about its feature like recently it's sequencer was down for some time which is offically mentioned by them [( click here)](https://docs.arbitrum.io/sequencer) and [here](https://github.com/ArbitrumFoundation/docs/blob/50ee88b406e6e5f3866b32d147d05a6adb0ab50e/postmortems/15_Dec_2023.md). Another **PUSH0** opcode is not supported by [arbitrum chain](https://docs.arbitrum.io/for-devs/concepts/differences-between-arbitrum-ethereum/solidity-support)




## 7. Hours spend

55 hours


## Highlights of the report 


1.Representing the smart contract working flow through the diagram. 

2.Extensive analysis of contract by contract and function by function analysis. 

3.Comparison table with other competitive protocol 

4.Best recommendation which could safely implemented.

### Time spent:
55 hours