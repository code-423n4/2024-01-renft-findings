### Summary

reNFT is a protocol that allows lenders to lend their NFT to renters for a price. reNFT uses external integrations like Safe and Seaport to make the protocol more dynamic and secure.

Instead of a simple renting model, reNFT utilizes seaport to create a dynamic renting model. Lenders can specify their duration of lend, their NFTs, and how much they are willing to get (range) through Seaport.

There are also two prominent types of rent, BASE and PAY. For BASE rentals, lenders will list their NFT for rent and get paid an ERC20 of their choice. For PAY rentals, lenders will list their NFTs and also pay the renter pro-rated amounts for every second that they rent the NFTs.

The creation of rentals, signature, and fulfillment of rent orders is done through Seaport. When an order is fulfilled, the NFTs will be sent to a Safe owned by the renter, and the ERC20 tokens will be sent to the payment Escrow contract. Note that the renter has to create a Safe in order to rent the NFT from reNFT.

Once the rental duration is over, the rented NFT will be sent back to the lender through the Safe contract and the ERC20 tokens will be disbursed accordingly.

For PAY order types, lenders can withdraw their rented NFT at any time, but they have to pay a pro-rated payment to the renter.

### Approach

- The protocol was hard to understand at first glance, and requires prerequisite knowledge about Safe and Seaport
- Before anything, I went to read up about [Safe](https://docs.safe.global/getting-started/readme) and their [Github](https://github.com/safe-global/safe-contracts/tree/main)
- I tried to find which functions in Safe is being used in the protocol (eg createProxyWithNonce, setup) and figure out how the protocol is integrating Safe into its contracts.
- Next, I read up on Seaport, mostly watching 0age videos on [Youtube](https://www.youtube.com/watch?v=YLWnaSymFHA) and some [Web Articles](https://pixelplex.io/blog/seaport-protocol-explained/)
- I tried to understand how an order is created, signed and fulfilled, and how Seaport is being integrated effectively. Also, I tried learning some keywords such as Items, Considerations, Zone.
- With my newfound knowledge of the external integration, I tried to navigate the codebase again, but was lost at the design pattern.
- I realised that I needed some [Default](https://github.com/fullyallocated/Default) framework knowledge.

- After all the learnings, I get a rough idea of the protocol structure and mechanism. There wasn't much to audit since a lot of mechanisms hinges on the admin, and will therefore fall under centralization risk. With that in mind, I decided to focus on PaymentEscrow.sol and Stop.sol, the contracts that handles the transfer of ERC20 tokens and NFTs (and also the entry point for stopping rentals).
- Finally, I looked at the test to better understand the flow of the protocol and get a better sensing of how token moves around.

### Codebase Quality Analysis and Review

##### PaymentEscrow.sol
- Settles the ERC20 payment from either the lender or renter through the BASE or PAY option.
- Fees are also settled in the contract
- settlePayment can only be called through Stop.sol
- The rest of the functions are protected, only can be called through proxy and permissioned actors
- freeze and upgrade can be quite concerning because it is irreversible. Exercise caution when using such functions.
- skim can only be used by the admin to withdraw fees and other tokens accidentally sent to the contract

##### Storage.sol
- stores the rentals (add and remove) called by Create.sol and Stop.sol respectively
- In addRentals, duplicated orderHash is not checked, but that may not be needed because it will be extremely hard to duplicate an orderHash with different parameters
- RemoveRentals does check for the existence of orderHash, and will delete the orderHash. Only Stop.sol can call `removeRentals()`, so the orderHash cannot be deleted easily.
- addRentalSafe acts like a nonce. Although there is no upper limit, it is pretty much impossible to reach uint256.max value.
- WhitelistDelegates and Extensions are centralization risks, so the admin have to pay close attention to the address that they are whitelisting.

PaymentEscrow and Storage contracts are modules. Modules are internal-facing contracts that store shared state across the protocol. They do not do anything by themselves, and interacts with policy in order to change the internal state. Modules can only be accessed through whitelisted Policy contracts, and have no dependencies of their own. Modules can only modify their own internal state.

##### Admin.sol
- sets the fees for the payment escrow, also able to call skim and freeze and upgrade
- Admin also can freeze and upgrade the storage.sol contract
- All functions that are used by admin.sol has been requested permission.

##### Factory.sol

`deployRentalSafe`
- Code is checked against the Safe contract. 
- threshold and owner validation is not needed as it is already done in safe (OwnerManager.setupOwners)
- Two Policies are added, Stop and Guard as an extension to the Safe contract. These Safe can be interacted through these two policies (Stop for stopping rental and Guard for Hooks)
- When safe.setup is called, all 8 parameters in Factory.sol correspond to the parameters in Safe.setup. Note that there is no payment token and amount because safe is designed to hold only ERC721 and ERC1155 token for the protocol.
- createProxyWithNonce is called with the correct parameters and datatype (_singleton, initalizer, saltNonce)
- Nonce is updated during addRentalSafe.

##### Guard.sol

- Protects the safe so that only policies can withdraw the contents inside the safe. In other words, lenders and renters cannot withdraw from the safe directly
- Controls how hooks are used for each safe. Uses updateHookPath and updateHookStatus from the storage contract to whitelist a hook.

##### Reclaimer.sol

- Is an abstract contract inherited by Stop.sol
- Used in conjunction with Stop.sol, to reclaim a lender's NFT after rental
- The lender must be an address or a contract that implements onERC721Received fallback, otherwise it cannot claim the NFT back
- `reclaimRentalOrder()` can only be called from the rentalWallet. This is done through delegateCall from the Safe contract to the Stop.sol contract, such that the address(this) is the Safe (wallet) address
- If rentalWallet address is different, the order hash will be different, so malicious users cannot spoof a reclaim rental option.
- Only works for ERC721 and ERC1155

##### Kernel.sol

- Cross checked with the [Default Framework](https://github.com/fullyallocated/Default)
- Kernel.sol contract is the same as the [Default.Kernal contract](https://github.com/fullyallocated/Default/blob/master/src/Kernel.sol)
- Purpose of the Kernal is administrative: grant/revoke role, install/upgrade modules, activate/deactivate policy, migrate kernels

### Mechanism Review

`Creating a Safe`
1. A renter calls Factory.deployRentSafe
2. owners and threshold is checked. Threshold must be lower than the number of owners
3. Two policies are initialized: Stop and Guard. Stop is for stopping of rentals and guard is for hooks. No other policies can be initialized after a safe is created. These two policies can call functions in the safe.
4. Safe.setup will be called. The safe only allows for NFTs in this protocol
5. SafeProxyFactory.createProxyWithNonce is called
6. Storage.addRentalSafe is called. This will increase the total safes by 1.

- At the end of the interaction, a renter will have a safe created. If a renter already has a safe, he cannot use it as part of the protocol, because STORE.addRentalSafe has to return true for the newly created safe address.
- The safe is well created. No other policies can be introduced by the renter himself. Also, the nonce will always be changing, preventing malleability. 
- Most of the checks is done in Safe itself, including repeated owners, threshold, salt etc. The Factory integrates the Safe well.

`Stopping a Rent`
1. Validate Rental
- If Order is Base, the items can only be withdrawn after the expiry date
- If Order is Pay, the lender can withdraw the items before the expiry date
2. Create Dynamic asset array
3. Remove Hooks
4. Reclaim Rented Items
- This is interesting, as it calls the safe `execTransactionFromModule` method.
- The process goes like this: EOA -(call)> Stop.sol -(call)> ModuleManager.sol (abstract contract inherited by Safe) -(delegatecall)> Reclaimer.sol (abstract contract inherited by Stop.sol)
- Through the delegatecall, address(this) becomes the safe and not the Stop.sol contract, although the msg.sender and msg.value is Stop.sol (but these values are not used)
- When reclaimRentalOrder is called, only the rentalWallet can be used to call.
- In the rental wallet, `execTransactionFromModule` cannot be called directly because the msg.sender must be part of a module (In this case, Stop is part of the module)
- The module is instantiated when the safe is created, and this safe is created by factory.sol, so stop.sol is part of the module of Safe, and thus can be used to call `execTransactionFromModule`
- Transfers ERC721 and ERC1155 tokens back to the lender
5. Settle the Payment
- Transfers ERC20 payments from escrow to respective recipients
- If Order == Pay, then ERC20 payments will be pro-rated
6. Remove Rentals from the Storage.sol contract

- Fees are not pro-rated when calling stop rent halfway with OrderType == Pay
- Low decimal tokens may be affected by pro-rata calculations (truncation to zero), allowing the lenders to not pay the renters
- Fees can be set at 100%
- The renter can blocklist ERC20 payments, resulting in lenders not able to get their NFTs back
- Lenders cannot directly claim their NFTs back. There's only one way to interact with the whole stopping a rental process, and that is through Stop.stopRent() (Or stopRentBatch).

`Using Payment Escrow`
- It is assumed that when the order is fulfilled by lender and rental, the ERC20 payment will be deposited into the PaymentEscrow and the NFT is deposited into the SAFE
- The use of a payment escrow is good because it prevents p2p trading which will open up a lot of attack vectors.
- Fees are calculated first before any payment is sent
- The admin can get the fees and any other tokens accidentally deposited into the payment escrow contract

### Centralization Risks

- The protocol places heavy emphasis on centralization. Policies are whitelisted, hooks are whitelisted, fees can be set up to 100%, upgrade and freeze of contract is done by the admin
- Without admin intervention, protocol can still work but the functionalities will be limited.

- Overall Centralization risk: High

### Architecture Review

##### `Design Patterns and Best Practices:`

Established design patterns, like modifiers are used properly. Interesting usage of the DEFAULT framework, splitting the architecture into modules and policies. Seems like everything is set up well, every policy contract has requestPermissions of the functions that they are using from the modules.

Good usage of errors. Clear and concise error messages.

##### `Code Readability and Maintainability:`

Code is hard to read because of 2 external integration, Seaport and Gnosis Safe, and also the usage of Default framework. A lot of understanding of proxy and delegatecall is needed to understand the protocol. Despite that, code is pretty easy to maintain, as there is not many entrance point and admin can easily upgrade / freeze contracts.

##### `Error Handling and Input Validation:`

Events and Error messages are easy to understand. Error messages are pretty intuitive. Try-catch statements are also used well. Inputs are validated well. Creating a rental, stopping a rental, admin fees all have good validation (eg no duplication of order has, no doubling of rental, no stopping of rental twice).

Validation of external integration is also well handled, like interacting with Seaport and Safe

##### `Interoperability and Standards Compliance:`

Complies well with Safe, ERC20, ERC721 and ERC1155. Also uses the UUPS EIP-1822 format. Hooks are also use to improve interoperability between external integrations

##### `Testing and Documentation:`

Test has decent coverage. There is no proper documentation, only some markdowns in the github itself. There should be a full overview of how everything works from start to end, how Seaport and Safe is integrated, how users can interact with the protocol and how users/lenders can benefit from the protocol.

##### `Upgradeability:`

Protocol is upgradeable. Uses EIP-1822.

##### `Dependency Management:`

High reliance on external libraries like OpenZeppelin, Seaport, Safe which increases attack vectors.

Overall, architecture from the protocol is complex but works well

- More documentation and overarching explanation of the protocol.
- More guidance on which functions are called by which actor 

### Time spent:
25 hours