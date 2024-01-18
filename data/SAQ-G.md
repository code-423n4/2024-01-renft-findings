## Summary

### Gas Optimization

no | Issue |Instances||
|-|:-|:-:|:-:|
| [G-01] | Avoid contract existence checks by using low level calls | 8 | - |
| [G-02] | <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES or ( -= ) | 2 | - |
| [G-03] | Multiplication/division by two should use bit shifting | 3 | - |
| [G-04] | Not using the named return variable when a function returns, wastes deployment gas | 1 | - |
| [G-05] | Can make the variable outside the loop to save gas | 5 | - |
| [G-06] | State varaibles only set in the Constructor should be declared Immutable | 1 | - |
| [G-07] | With assembly, .call (bool success)  transfer can be done gas-optimized | 1 | - |
| [G-08] | Using ERC721A instead of ERC721 for more gas-efficient | 8 | - |
| [G-09] | Non efficient zero initialization | 2 | - |
| [G-10] | Pre-increment and pre-decrement are cheaper than +1 ,-1 | 5 | - |
| [G-11] | Use assembly for loops | 3 | - |
| [G-12] | do-while is cheaper than for-loops when the initial check can be skipped | 5 | - |
| [G-13] | Cache external calls outside of loop to avoid re-calling function on each iteration | 3 | - |
| [G-14] | Gas savings can be achieved by changing the model for assigning value to the structure 123 gas | 1 | - |
| [G-15] | Use assembly in place of abi.decode to extract calldata values more efficiently | 2 | - |


## Gas Optimizations  

## [G-01] Avoid contract existence checks by using low level calls		


```solidity
file:

102        (bool success, bytes memory data) = token.call(
103            abi.encodeWithSelector(IERC20.transfer.selector, to, value)

116            revert Errors.PaymentEscrowModule_PaymentTransferFailed(token, to, value);

268                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()

279                    revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L116C1-L116C87


```solidity
file: /packages/Reclaimer.sol

33        IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier);

74            revert Errors.ReclaimerPackage_OnlyDelegateCallAllowed();

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33C1-L33C89


```solidity
file: /packages/Signer.sol

112        bytes32 digest = _DOMAIN_SEPARATOR.toTypedDataHash(payloadHash);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L112C1-L112C73


```solidity
file:/policies/Create.sol

124         ISafe(address(this)).enableModule(_stopPolicy);

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L124


## [G-02] <X> += <Y> COSTS MORE GAS THAN <X> = <X> + <Y> FOR STATE VARIABLES or ( -= )


```solidity
file: /modules/PaymentEscrow.sol

306        balanceOf[token] += amount;

294        balanceOf[token] -= amount;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L306C1-L306C36


## [G-03] Multiplication/division by two should use bit shifting

<x> * 2 is the same as <x> << 1. 
While the compiler uses the SHL opcode to accomplish both, the version that uses multiplication incurs an overhead of 20 gas due to JUMPs to and from a compiler utility function that introduces checks which can be avoided by using unchecked {} around the division by two.

<x> / 2 is the same as <x> >> 1. 
While the compiler uses the SHR opcode to accomplish both, the version that uses division incurs an overhead of   20 gas due  to JUMPs to and from a compiler utility function that introduces checks which can be avoided by using unchecked {} around the division by two.


```solidity
file: /modules/PaymentEscrow.sol

90        return (amount * fee) / 10000;

138        uint256 numerator = (amount * elapsedTime) * 1000;

142        renterAmount = ((numerator / totalTime) + 500) / 1000;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L90C1-L90C39


## [G-04] Not using the named return variable when a function returns, wastes deployment gas


```solidity
file: /Create2Deployer.sol

94         return address(uint160(uint256(addressHash)));

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L94C1-L94C55


## [G-05] Can make the variable outside the loop to save gas

Consider making the stack variables before the loop which gonna save gas

```solidity
file:/modules/PaymentEscrow.sol

233            Item memory item = items[i];

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L233C1-L233C41


```solidity
file: /src/packages/Accumulator.sol

122            RentalId rentalId;

123            uint256 amount;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L122C1-L123C28


```solidity
file: /policies/Create.sol

263            SpentItem memory offer = offers[i];

377            ReceivedItem memory consideration = considerations[i];

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L263C1-L263C48


## [G-06] State varaibles only set in the Constructor should be declared Immutable

```solidity
file: /Kernel.sol

34        kernel = kernel_;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L34C1-L34C26


## [G-07]  With assembly, .call (bool success)  transfer can be done gas-optimized

```solidity
file: /modules/PaymentEscrow.sol

102  (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(IERC20.transfer.selector, to, value)
        );

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L102C6-L104C11


## [G-08] Using ERC721A instead of ERC721 for more gas-efficient 

ERC721A is a proposed extension to the ERC721 standard that aims to improve gas efficiency and reduce the cost of deploying and using non-fungible tokens (NFTs) on the Ethereum blockchain. 

```solidity
file: /packages/Reclaimer.sol

33        IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier);

94            if (item.itemType == ItemType.ERC721)

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33C1-L33C89


```solidity
file: /policies/Create.sol

269                itemType = ItemType.ERC721;

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L269C1-L269C44


## [G-09] Non efficient zero initialization
 
Solidity does not recognize null as a value, so uint variables are initialized to zero. Setting a uint variable to zero is redundant and can waste gas.

```solidity
file: /modules/PaymentEscrow.sol

231        for (uint256 i = 0; i < items.length; ++i) {

341        for (uint256 i = 0; i < orders.length; ++i) {    

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L231C1-L231C53


## [G-10] Pre-increment and pre-decrement are cheaper than +1 ,-1

```solidity
file: /policies/Create.sol

184                uint256(keccak256(abi.encode(STORE.totalSafes() + 1, block.chainid)))

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#184


```solidity
file: /src/Kernel.sol

431        getPolicyIndex[policy_] = activePolicies.length - 1;

445            getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1;

469        Policy lastPolicy = activePolicies[activePolicies.length - 1];

601            Policy lastPolicy = dependents[dependents.length - 1];

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L431C1-L431C61


## [G-11] Use assembly for loops

In the following instances, assembly is used for more gas efficient loops.
The only memory slots that are manually used in the loops are scratch space (0x00-0x20), the free memory pointer (0x40), and the zero slot (0x60).
This allows us to avoid using the free memory pointer to allocate new memory, which may result in memory expansion costs.

```solidity
file: /modules/PaymentEscrow.sol

341       for (uint256 i = 0; i < orders.length; ++i) {
            // Settle all payments for the order.
            _settlePayment(
                orders[i].items,
                orders[i].orderType,
                orders[i].lender,
                orders[i].renter,
                orders[i].startTimestamp,
                orders[i].endTimestamp
            );
        }
    }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L341C1-L352C6


```solidity
file: /packages/Signer.sol

176    for (uint256 i = 0; i < order.hooks.length; ++i) {
            // Hash the hook.
            hookHashes[i] = _deriveHookHash(order.hooks[i]);
        }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L176C4-L179C10


```solidity
file: /policies/Create.sol#

337    for (uint256 i; i < considerations.length; ++i) {
            // Get the consideration item.
            ReceivedItem memory consideration = considerations[i];

            // Only process an ERC20 item.
            if (!consideration.isERC20()) {
                revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                    consideration.itemType
                );
            }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L337C6-L346C14


## [G-12] do-while is cheaper than for-loops when the initial check can be skipped


for (uint256 i; i < len; ++i){ ... } -> do { ...; ++i } while (i < len);

```solidity
file: /modules/PaymentEscrow.sol

231        for (uint256 i = 0; i < items.length; ++i) {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L231C1-L231C53


```solidity
file: /packages/Accumulator.sol

120        for (uint256 i = 0; i < rentalAssetUpdateLength; ++i) {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L120C1-L120C64


```solidity 
file: /packages/Reclaimer.sol

90        for (uint256 i = 0; i < itemCount; ++i) {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L90


```solidity
file: /policies/Stop.sol

205         for (uint256 i = 0; i < hooks.length; ++i) {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L205C1-L205C53


```solidity
file: /Kernel.sol

566        for (uint256 i = 0; i < reqLength; ++i) {
            // Set the permission for the keycode -> policy -> function selector.
            Permissions memory request = requests_[i];
            modulePermissions[request.keycode][policy_][request.funcSelector] = grant_;

            emit Events.PermissionsUpdated(
                request.keycode,
                policy_,
                request.funcSelector,
                grant_
            );
        }
    }

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L566C1-L578C6


## [G-13] Cache external calls outside of loop to avoid re-calling function on each iteration

```solidity
file: /modules/PaymentEscrow.sol

266                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()

279                    revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L268C1-L268C88


```solidity
file: /policies/Create.sol

343                revert Errors.CreatePolicy_SeaportItemTypeNotSupported(

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L343C1-L343C72


## [G-14] Gas savings can be achieved by changing the model for assigning value to the structure 123 gas


Change this structName a = structName({item1: val1,item2: val2}); ==> structName a; a.item1 = val1; a.item2 = val2;

```solidity
file: /policies/Create.sol

228       rentalItems[i + startIndex] = Item({
                itemType: itemType,
                settleTo: SettleTo.LENDER,
                token: offer.token,
                amount: offer.amount,
                identifier: offer.identifier
            });

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L228C12-L234C16


## [G-15] Use assembly in place of abi.decode to extract calldata values more efficiently

Instead of using abi.decode, we can use assembly to decode our desired calldata values directly. This will allow us to avoid decoding calldata values that we will not use.
For more details on how to implement this, check the following report

```solidity
file: /modules/PaymentEscrow.sol

115        if (!success || (data.length != 0 && !abi.decode(data, (bool)))) {

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L115C1-L115C75


```solidity
file: /src/policies/Create.sol

737      (RentPayload memory payload, bytes memory signature) = abi.decode(
            zoneParams.extraData,
            (RentPayload, bytes)
        );

```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L737C3-L740C11

