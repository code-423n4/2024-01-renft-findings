## Audit Scope

reNFT protocol is built on the idea of facilitating ERC721/ERC1155 tokens rental, where it's built on top of SeaPort protocol and Gnosis SAFE wallets where the rented items are going to be reseted in.

Th audit scope consists of 12 contracts with ~ 1905 SLoC.

## Methodlogy

Methodology adopted during the audit:

1. Gaining a high-level overflow of the protocol and what it intends to do.
2. Understanding SeaPort orders addition/fulfilling orders process and how the protocol integrates with it.
3. Understanding the architecture adopted by the protocol and how the contracts are connected with each other, then started with the entry point of the protocol which is `validateOrder`.
4. Main focus was to find flows in the following areas; where the invariants might be broken:
   - Integration with SeaPort.
   - Handelling NFTs ownership.
   - Balances and payment calculations are always ensured to follow the intended buisness logic.
   - Signatures replays.
   - Any possibility of hash collisions.
   - Invalid checks that may cause revert on vital actions (such as revert on hooks execution when a rental is stopped).

## Invariant Violations

- The main invariant that the protocol is more concerned about is preserving the rented asset in the renter SAFE wallet and safely returning the assets back to the lender after the rental is over, but it was noticed that this invariant (where the rented tokens are at risk of being stuck in the renter SAFE wallet) can be broken in the following cases:

  1.  If -upon `stopRental` function invocation- any of the on-stop hooks reverted or became unsupported.
  2.  Knowing the the ERC20 payment tokens used in the protocol are the same ERC20 tokens used in SeaPort, and one of these tokens is USDC where it has a blocklist, then if the order type is a BASE order and the lender that's going to receive the USDC payment is blocklisted, or if the order is PAY and any of the lender or renter is blocklisted (or became after the fullfillment of the order) by the USDC, then the NFT will be stuck

  3.  Knowing that USDC is an upgradeable contract that might charge fees in the future, which will result in partially stuck NFTs in the wallets (partially as anyone can send payments directly to the `PaymentEscrow` contract to compensate for the deducted fees and rescue the stuck rented assets).

  4.  And one important thing is if the recipient is a multisig wallet (contract) that doesn't implement `onERC721Received` or `onERC1155Received`; then they can't receive their rented tokens via `safeTransfer` and the assets will be stuck **permanently** in the renter SAFE wallet (this is a different issue from the one reported by the winning bot: where the SAFE wallet is unable to receive NFTs or ERC1155 tokens as the don't inherint the OZ ERC721Holder ERC1155Holder contracts).

## Other Systematic Risks

- The protocol uses `EIP712` specifications to create hashes for orders and orders metadata, but it was noticed that these hashes are not compliant with the specifications.

- In cases of hardforks, the protocol will be disabled (temporarily until upgrading the `Create` contract) as it uses an immutable to save the EIP712 `_DOMAIN_SEPARATOR` that encodes the `chain.id` at the time of the deployment; so after a hardfork, signatures sent to verify SeaPort calls to the `validateOrder` will not be validated and the fulfilling orders functionality will not be working anymore.

## Centralization Risks

- On the level of the on-chain contracts: almost centralization issues are limited, as the executor role is the main role in the protocol that can do upgrades, migrations, enabling and disabling policies, but there's some noted issues:
  1.  If any of the on-stop hooks is disabled, then the rental order that uses that hook can never be stopped, and this will result in locking the rented assets in the renter wallet.
  2.  If the kernel executor upgrades the contracts without checking if all orders are stopped; this will result in stuck rented assets.

## User Flow

![User Flow](https://drive.google.com/uc?id=1X4nGMsQrZjLDyCvrI4PInha4uUt1U0mo)

**Starting and Fulfilling a Rental Order**
If Alice has an NFT that she would like to lend -> she will have to interact with the reNFT frontend to create a rentalOrder -> then the order is checked in the server-side and validated -> then this order is listed on SeaPort -> Now Bob wants to rent this NFT -> he will interact with reNFT with a signed order -> the reNFT server-side will interact with SeaPort to fulfill the order -> Then the rented assets will be moved to a SAFE wallet created and whitlisted for the renter

**Stoppinga a Rental Order**
If the time of the rental is over or if Alice decided to stop the rental (if it's a PAY order) -> `stopRental` is called where the rented assets are returned back to Alice -> and the payment is paid for the intended parties (for Bob if it's a BASE order, and for Bob and Alice if it's a PAY order that hasn't ended yet).

note : to ease the following the flow, Bob acts as he interacts with SeaPort directly, while he is actually interacting with reNFT front-end, and the reNFT server-side will interact with SeaPort to fulfill the order.

## Architecture Review

![Simplified Architecture](https://drive.google.com/uc?id=13Coa3scBhAQmZYmmq-H_elDLhtG5cScK)

- The protocol adopts the **default framework** architectural model, where it separates contracts into internal modules (back-end) and external policies (user facing contracts/front-end) for flexible, modular protocol design, where modules manage specific data models independently, while policies handle inbound calls and route updates to Modules, acting as intermediaries.

- This separation of data management (what) from business logic (why) enhances flexibility and immutability, simplifying protocol development and any future features addition.

- All the contracts are managed by the Kernel contract, which is a registry that manages all the changes and upgrades of the protocol: adding/removing policies and modules.

## Enahancement

In general, the codebase is robust and follows the security best practices and implementations, but it would be a safe play if a pulling mechanism is adopted instead of pushing mechanism when the rented assets are returned and when the payments are paid for the intended parties, as this would prevent any risks related to denial-of-service due to external un-expected behaviours.

Also following the EIP712 specifications when creating hashes would prevent any integration issues with SeaPort protocol.

## Time Spent

Approximately 25 hours ; divided between reading the documentation, manually reviewing the codebase, and documenting my findings.


### Time spent:
25 hours