## Protocol Overview
ReNFT protocol facilitates generalized collateral-free NFT rentals built on top
of Gnosis Safe and Seaport.

To give an example, imagine Alice has gaming NFTs. She signs seaport order typed
data and thus signals that she is happy to lend out these assets. Now, Bob would
love to use the NFTs in the game. He finds Alice's listing and rents. This is
where these contracts come into force. A gnosis safe is created for Bob where
assets he rented get sent to. There is a gnosis module that disallows Bob to
move out the assets from his smart contract wallet. He is now free to use the
NFTs in-game.
![Imgur](https://i.imgur.com/SvG43lX.png)

## Contracts overview
The contracts in scope can be
categorized into four main groups:
1. Modules
Modules are internal-facing contracts that store shared state across the
protocol.
- Payment Escrow:
Module dedicated to escrowing rental payments while rentals are active. When
rentals are stopped, this module will determine payouts to all parties and a
fee will be reserved to be withdrawn later by a protocol admin.
- Storage:
Module dedicated to maintaining all the storage for the protocol. Includes
storage for active rentals, deployed rental safes, hooks, and whitelists.
![Imgur](https://i.imgur.com/58vW8kl.png)

2. Policies
Policies are external-facing contracts that receive inbound calls to the
protocol, and route all the necessary updates to data models via Modules.
- Admin:
Acts as an interface for all behavior in the protocol related to admin logic.
Admin duties include fee management, proxy management, and whitelist
management.
- Create:
Acts as an interface for all behavior related to creating a rental. This is
the entrypoint for creating a rental through the protocol, which only Seaport
contracts can access.
- Factory:
Acts as an interface for all behavior related to deploying rental safes.
Deploys rental safes using gnosis safe factory contracts.
- Guard:
Acts as an interface for all behavior related to guarding transactions that
originate from a rental wallet. Prevents transfers of ERC721 and ERC1155
tokens while a rental is active, as well as preventing token approvals and
enabling of non-whitelisted gnosis safe modules.
- Stop:
Acts as an interface for all behavior related to stopping a rental. This policy
is also a module enabled on all rental safe wallets, and has the authority to
pull funds out of a rental wallet if a rental is being stopped.
![Imgur](https://i.imgur.com/PFSwK9U.png)

3. Packages
Packages are small, helper contracts dedicated to performing a single task which
are imported by other core contracts in the protocol.

- Accumulator:
Package that implements functionality for managing dynamically allocated data
struct arrays directly in memory. The rationale for this was the need for an
array of structs where the total size is not known at instantiation.
- Reclaimer:
Retrieves rented assets from a wallet contract once a rental has been stopped,
and transfers them to the proper recipient. A delegate call from the safe to
the reclaimer is made to pull the assets.
- Signer:
Contains logic related to signed payloads and signature verification when
creating rentals.
![Imgur](https://i.imgur.com/8vWhLcM.png)

4. General
These are general-purpose contracts which are agnostic to the core functionality
of the protocol.

- Create2 Deployer:
Deployment contract that uses the init code and a salt to perform a
deployment. There is added cross-chain safety as well because a particular
salt can only be used if the sender's address is contained within that salt.
This prevents a contract on one chain from being deployed by a non-admin
account on another chain.
- Kernel:
A registry contract that manages a set of policy and module contracts, as well
as the permissions to interact with those contracts. Privileged admin and
executor roles exist to allow for role-granting and execution of kernel
functionality which includes adding new policies or upgrading modules.
![Imgur](https://i.imgur.com/T6u5E5s.png)

## Creating a Rental

Creating a rental order involves constructing and signing a valid seaport order with a special data payload so that our protocol's zone contract is invoked during the processing of the order.

### Creating the Seaport Order

When an EOA wants to list a rental order, they must first create and sign a seaport order. 


Seaport order components are structured as follows:
An order contains eleven components: an offerer, a zone (or account that
can cancel the order or restrict who can fulfill the order depending on
the type), the order type (specifying partial fill support as well as
restricted order status), the start and end time, a hash that will be
provided to the zone when validating restricted orders, a salt, a key
corresponding to a given conduit, a counter, and an arbitrary number of
offer items that can be spent along with consideration items that must
be received by their respective recipient.
struct OrderComponents {
    address offerer;
    address zone;
    OfferItem[] offer;
    ConsiderationItem[] consideration;
    OrderType orderType;
    uint256 startTime;
    uint256 endTime;
    bytes32 zoneHash;
    uint256 salt;
    bytes32 conduitKey;
    uint256 counter;
}

The items to pay particular attention to here are the `zone` and `zoneHash` fields.

For our protocol, the `zone` address will always be the address of the [Create Policy](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol).

Specifying a zone is crucial because a seaport order will not be routed through the protocol unless the `zone` field is given the right address.

The `zoneHash` is a hashed value of data which describes the unique values of this particular rental. After this order has been signed, a counterparty (the fulfiller) will pass the unhashed data which makes up the zone hash into the Seaport fulfillment function to prove to the protocol that both the lender and the renter of the order have agreed on the rental terms. 
![Imgur](https://i.imgur.com/lCSg4kS.png)

### Constructing the Zone Hash

A zone hash is the EIP-712 hashed version of the following struct: 

/**
 * @dev Order metadata contains all the details supplied by the offerer when they sign an
 *      order. These items include the type of rental order, how long the rental will be
 *      active, any hooks associated with the order, and any data that should be emitted
 *      when the rental starts.
 */
struct OrderMetadata {
    // Type of order being created.
    OrderType orderType;
    // Duration of the rental in seconds.
    uint256 rentDuration;
    // Hooks that will act as middleware for the items in the order.
    Hook[] hooks;
    // Any extra data to be emitted upon order fulfillment.
    bytes emittedExtraData;
}

> To see how an OrderMetadata struct is converted into a EIP-712 typehash, please see the [Signer Package](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol).

`orderType`: When a lender wants to create a rental, they must choose an order type. There are 3 order types supported by the protocol, but only 2 are external and can be used by lenders. 
- `BASE`: This order type describes an order in which the lender will construct a seaport order that contains at least one ERC721 or ERC1155 offer item, and at least one ERC20 consideration item. A lender would select this order when they want to be paid by a renter in exchange for lending out their asset(s) for a specific amount of time.
- `PAY`: This order type describes an order in which the lender wishes to *pay* the renter for renting out their asset. This order must contain at least one ERC721 or ERC1155 offer item, and at least one ERC20 offer item. It must contain 0 consideration items. This may sound counter-intuitive but the rationale is that some lenders may get benefit (tokens, rewards, etc) from allowing others to interact with contracts (on-chain games, etc) with their assets to extract some type of value from the lended asset. 
- `PAYEE`: This order type cannot be specified by a lender and should result in a revert if specified. As such, it is not used during rental creation. `PAYEE` orders act as mirror images of a `PAY` order. In other words, a `PAYEE` order has 0 offer items, and should specify the offer items of the target `PAY` order as its own consideration items, with the proper recipient addresses.
![Imgur](https://i.imgur.com/HFWKmVv.png)

`rentDuration`: This is the total duration of the rental in seconds.

`hooks`: These are the hooks which will be specified for the rental. Please see the documentation on [hooks](./hooks.md) for more info.

`emittedExtraData`: This is any extra data that the lender wishes to emit once a rental has been fulfilled.

After the `OrderMetadata` has been constructed, its hash can be constructed using a convenience function which exists on the [Create Policy](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol):

/**
 * @notice Derives the order metadata EIP-712 compliant hash from an `OrderMetadata`.
 *
 * @param metadata Order metadata converted to a hash.
*/
function getOrderMetadataHash(
    OrderMetadata memory metadata
) external view returns (bytes32) {
    return _deriveOrderMetadataHash(metadata);
}

The resulting value is what is used as the zone hash.

### Finalizing the Order

Once the order is properly constructed, it can be signed by the offerer and await fulfillment. For examples on order construction, you can view the solidity test cases specified [here](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/integration/Rent.t.sol).

## Fulfilling a Rental


Fulfilling a rental order involves interacting with one of Seaport's fulfillment functions to process a signed order. The fulfillment functions used can be found [here](https://github.com/re-nft/seaport-core/blob/3bccb8e1da43cbd9925e97cf59cb17c25d1eaf95/src/lib/Consideration.sol), and are listed below for convenience: 
- `fulfillAdvancedOrder`
- `fulfillAvailableAdvancedOrders`
- `matchAdvancedOrders`


### Calling a Fulfillment Function

Signed orders can be fulfilled by the protocol in a few ways depending on how many orders are being filled at once, and what type of orders are being fulfilled. 

`BASE` orders: A single `BASE` order can be fulfilled using `fulfillAdvancedOrder()`. Multiple `BASE` orders can be fulfilled using `fulfillAvailabeAdvancedOrders()`.

`PAY` orders: Both single and multiple `PAY` orders can be fulfilled using Seaport's `matchAdvancedOrders()`.

For examples of order fulfillment, you can look at the test engine code [here](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/test/fixtures/engine/OrderFulfiller.sol).
![Imgur](https://i.imgur.com/UtsDTjk.png)

## Codebase strenghs

- Codebase is well written and respects the solidity best practices.
- Comments and natspec were really helpfull, they were really detailled and self-explanatory.
- Redondants codes are refactored into modifiers.
- Available documentation is well written and self-explanatory.
- Codebase use Kernel Module architecture , which is a strong architecture.

## Codebase weaknesses

- There is almost no documentation about the protocol
- creating a rental is not well explained, this led to imense questions in the discord chat
- The tests are not exhaustive as they should be, this can be improved.

## Audits approach

- Day 1: read the docs and understand the protocol.
- Day 2: Delve deep into the codebase to get a general code architecture understanding.
- Day 3: Delve more deeper into smarts contracts to understand more the code.
- Day 4: Compile the findings and write the report.

### Time spent:
20 hours