### Low Risk

| Count | Title |
| --- | --- |
| [L-01] | CryptoPunk NFTs will be stolen |
| [L-02] | Kernel deployment can be frontrunned  |
| [L-03] | ERC721Pausable tokens can block reclaiming |
| [L-04] | Some NFTs will return the floor price when burned |
| [L-05] | No validation if Order.orderType and OrderMetadata.orderType are equal |

| Total Low Risk Issues | 5 |
| --- | --- |

### Non-Critical

| Count | Title |
| --- | --- |
| [NC-01] | Typos |
| [NC-02] |  CheckTransaction can be improved  |
| [NC-03] | setActiveStatus can implement check for status beforehand |

| Total Non-Critical Issues | 3 |
| --- | --- |

## Low Risks

## [L-01] CryptoPunk NFTs will be stolen

**Issue Description:**

CryptoPunks collection was created before EIP712 was introduced and has an additional function used to offer punk for sale to certain addresses:
`offerPunkForSaleToAddress`. 

```solidity
function offerPunkForSaleToAddress(uint punkIndex, uint minSalePriceInWei, address toAddress) {
    if (!allPunksAssigned) throw;
    if (punkIndexToAddress[punkIndex] != msg.sender) throw;
    if (punkIndex >= 10000) throw;
    punksOfferedForSale[punkIndex] = Offer(true, punkIndex, msg.sender, minSalePriceInWei, toAddress);
    PunkOffered(punkIndex, minSalePriceInWei, toAddress);
}
```

CryptoPunk NFT renter can call freely this function because Guard is not handling it and pass 1 wei as a price.

Then after rental period is terminated he will still have approval to buy it without any interruptions.

**Recommendation:**

There is no universal mitigation for all these non-standard NFT implementations but care must be taken when dealing with such tokens and especially CryptoPunks as they are the foundation of the whole NFT ecosystem.

## [L-02] Kernel deployment can be frontrunned

**Issue Description:**

Kernel relies on the deployer to specify addresses of the `Admin` and `Executor`.

```solidity
//@audit frontrun
  constructor(address _executor, address _admin) {
      executor = _executor;
      admin = _admin;
  }
```

 There is also functionality to migrate kernel to a new implementation. 

```solidity
function executeAction(Actions action_, address target_) external onlyExecutor {
  ...More code
      } else if (action_ == Actions.MigrateKernel) {
          ensureContract(target_);
          _migrateKernel(Kernel(target_));
			}
  ...More code
      emit Events.ActionExecuted(action_, target_);
  }
```

That way of setting roles which are critical for the protocol opens frontrun possibility by a person who will set himself as an `executor` since this is the person with most privileges across the `reNFT` contract.

**Recommendation:**

Consider adding `Ownable2Step` to the `Kernel` and assign the owner from there, after that have a separate `onlyOwner` function to set and change the `executor` role.

## [L-03] ERC721Pausable tokens can block reclaiming

**Issue Description:**

Since [ERC721Pausable](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/extensions/ERC721Pausable.sol) is an extension to the EIP712 standard, there will be many tokens that are going to implement it. 

One possible use case of renting an NFTs will be to show proof of an ownership to an specific game or organisation, for example Bored Ape or Axie Infinity game token. 

If pausable token is being renter there is nothing to stop the renter from **pausing the transfers of these NFTs**.

He will be able to continue to use it, while the rent will be permanent and there will be no way to recover the rented items from the `Stop` module.

**Recommendation:**

Consider adding `pause`/`unpause` selectors to the `Guard::checkTransaction`, that will successfully mitigate the problem.

## [L-04] Some NFTs will return the floor price when burned

**Issue Description:**

There are certain NFTs that will return the floor price to the owner when burned.
One example is the [Burnables](https://opensea.io/collection/burnables-nft?embed%5B0%5D=test&embed%5B1%5D=test(.(%22()%27.%2C%2C). 
We can verify this statement from the description of the collection:

> The first-ever refundable NFT. The owner of the token can burn their NFT at any time and have the original mint price sent to their wallet.
> 

Users who have such a token and are non-technical can also rent their NFTs. Then renter who spotted it can rent it for short period and burn it to receive the **mint price** of the `NFT`.

This will result in a direct profit for renter, and big loss for the lender because rent payment won’t be distributed as well.

**Recommendation:**

Add burn function selector which will remove all the malicious use cases for such tokens. 

## [L-05] No validation if `Order.orderType` and `OrderMetadata.orderType` are equal

**Issue Description:**

The `OrderMetadata` hash, generated in `_deriveOrderMetadataHash()`, is missing two crucial members: `orderType` and `emittedExtraData`. When fulfilling an order, Seaport calls `Create::validateOrder()`, performing necessary logic on zones, conversion, and checks. The problem arises because the Lender sets `Order.orderType` initially, but the Renter can later provide a different value in `OrderMetadata`. As the hash doesn't consider `orderType`, `_isValidOrderMetadata()` passes, causing items to be converted based on the Renter's `OrderMetadata.orderType`, leading to potential inconsistency. For example, if the Lender sets `PAY` as the `Order.orderType`, the Renter can pass `BASE` in `OrderMetadata`, resulting in inconsistency.

As all `PAY` orders are provided with `PAYEE` by the reNFT backend and executed collectively, the second `validateOrder()` call in `_processBaseOrderConsideration()` reverts, because it will try to convert ERC721, but the function only work with ERC20. However, this scenario has no impact whatsoever, except for the pre-existing inconsistency.

**Recommendation:**

To avoid the inconsistency, consider checking whether `Order.orderType` equals `OrderMetadata.orderType` initially in `validateOrder()`. This step can help skip unnecessary computations or align all logic based on `Order.orderType` rather than relying on the fulfiller's `OrderMetadata.orderType`.

## Non-Critical

## **[N‑01] Typos**

**Issue Description:**

Various typos are presented in the codebase that should be fixed to improve the overall quality of the code and make the understanding easier:

as a a kernel → as a kernel

[Kernel.sol#L28](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L28)

```diff
- @dev Instantiate this contract as a a kernel adapter. When using a proxy, the kernel address
+ @dev Instantiate this contract as a kernel adapter. When using a proxy, the kernel address
```

as a a kernel → as a kernel

[Kernel.sol#L66](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L66)

```diff
- @dev Instantiate this contract as a a kernel adapter. When using a proxy, the kernel address
+ @dev Instantiate this contract as a kernel adapter. When using a proxy, the kernel address
```

seaport → Seaport

[Create.sol#L408](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L408)

```diff
- @param considerations Array of seaport consideration items.
+ @param considerations Array of Seaport consideration items.
```

## **[N‑02] `CheckTransaction` can be improved**

**Issue Description:**

`CheckTransaction` is used to prevent transfers and approvals of rented assets originated from safe wallet which has enabled this guard. 

Special case are the `batch` transactions: `safeTransferBatch`, `safeTransferFromBatch`. They are totally forbidden from the `Guard` even if they don’t contain rented assets, due to the complexity that they add - needing to extract the tokenId offset for every token passed in the ids array:  

```solidity
} else {
        // Revert if the `setApprovalForAll` selector is specified. This selector is
        // shared between ERC721 and ERC1155 tokens.
        if (selector == shared_set_approval_for_all_selector) {
            revert Errors.GuardPolicy_UnauthorizedSelector(
                shared_set_approval_for_all_selector
            );
        }

        // Revert if the `safeBatchTransferFrom` selector is specified. There's no
        // cheap way to check if individual items in the batch are rented out.
        // Each token ID would require a call to the storage contract to check
        // its rental status.
        if (selector == e1155_safe_batch_transfer_from_selector) {
            revert Errors.GuardPolicy_UnauthorizedSelector(
                e1155_safe_batch_transfer_from_selector
            );
        }

        // Revert if the `setGuard` selector is specified.
        if (selector == gnosis_safe_set_guard_selector) {
            revert Errors.GuardPolicy_UnauthorizedSelector(
                gnosis_safe_set_guard_selector
            );
        }
    }
```

This checks can be simplified by placing all the `batch` transactions into the `else if`:

```solidity
else if (
            selector == shared_set_approval_for_all_selector || selector == e1155_safe_batch_transfer_from_selector
                || selector == gnosis_safe_set_guard_selector
        ) {
            revert Errors.GuardPolicy_UnauthorizedSelector(selector);
        }
```

This will make the whole function shorter and easier to spot that this types of transactions are not supported for the entire safe wallet.

## **[N‑03] `setActiveStatus` can implement check for status beforehand**

**Issue Description:**

`setActiveStatus` function can check for the status of the policy before setting it, that will improve the flow:

```diff
function setActiveStatus(bool activate_) external onlyKernel {
+       require(activate_ != isActive, "Cannot re-set same status");
        isActive = activate_;
    }
```