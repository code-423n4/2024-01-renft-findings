### Low-01: Consider adding a realistic cap for protocol fees, current fee percentage can be set to 100%

The protocol fee is a percentage of the total ERC20 payment for a rental order. The fee will be deducted from ERC20 payment amount at the time of settlement of a rental order. 

The problem is the fee percentage can be set to 100% based on current implementation, which means the offerer or fulfiller will get zero ERC20 payment at the end of a rental order. 

```solidity
//src/modules/PaymentEscrow.sol
    function setFee(uint256 feeNumerator) external onlyByProxy permissioned {
        // Cannot accept a fee numerator greater than 10000.
        //@audit fee can be set to 100%
|>      if (feeNumerator > 10000) {
            revert Errors.PaymentEscrow_InvalidFeeNumerator();
        }
        // Set the fee.
        fee = feeNumerator;
    }

    function _settlePayment(
        Item[] calldata items,
        OrderType orderType,
        address lender,
        address renter,
        uint256 start,
        uint256 end
    ) internal {
...
                if (fee != 0) {
                    // Calculate the new fee.
                    uint256 paymentFee = _calculateFee(paymentAmount);

                    // Adjust the payment amount by the fee.
                    //@audit when fee is set to 100%, paymentAmount will be 0
|>                  paymentAmount -= paymentFee;
                }
```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L382)

**Recommendations:**
Consider adding a realistic cap percentage variable.

### Low-02: Accumulator.sol can be a library contract to reduce the inheritance chain.

Accumulator.sol is currently an abstract contract that needs to be inherited. But Accumulator.sol has no virtual functions to be overridden, and it only contains pure functions.

Accumulator.sol contains `_insert()` and `_converToStatic()`, which can be considered utility functions that manage dynamic bytes arrays. Also, `_insert()` and `_converToStatic()` are not virtual and cannot be overridden. 

In addition, Accumulator.sol's utility functions are also reused in multiple contracts such as Create.sol and Stop.sol.

All of the above suggests that it's better to convert Accumulator.sol into a library contract to reduce contract inheritance chains and also for code reuse. 

```solidity
//src/packages/Accumulator.sol
    function _insert(
        bytes memory rentalAssets,
        RentalId rentalId,
        uint256 rentalAssetAmount
    ) internal pure {
...

    function _convertToStatic(
        bytes memory rentalAssetUpdates
    ) internal pure returns (RentalAssetUpdate[] memory updates) {
...
```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L32-L36)
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L96-L98)

**Recommendations:**
Consider converting Accumulator.sol as a library contract.

### Low-03: A hook that is only enabled to run on start, will DOS the `stopRent()` flow

In the protocol, a rental order can have as many hooks as needed, and the entire list of hooks of a rental order will be run both at rental order creation (Create.sol - `validateOrder()`) and at rental stopping (Stop.sol - `stopRent()`).

And a hook can be authorized to run at different phases throughout a rental order. For example, `hookOnStart`, `hookOnTransaction` and `hookOnStop`. Each hook that is registered in Storage.sol by the protocol admin, might have a different combination of these options enabled through their bitmap value(`uint8 enabled`). 

A problem will occur if a hook is only intended to run at the start of a rental order creation. There can be two scenarios: (1) The hook contract implements `onStart()`, `onTransaction()`, and `onStop()` interface but only has an implementation for `onStart()`. And the protocol admin only enabled `hookOnStart` in Storage.sol; (2) The hook only implements `onStart()` methods, and do not have `onTransation()` or `onStop()` methods.

In both scenarios, `validateOrder()` will pass and the rental order will be created. However, they will revert when `stopRent()` is called at the end of the rental order. In (1), revert happens at Stop.sol - `_removeHooks()`- `if (!STORE.hookOnStop(target)) ` checks, where the hook's bitmap is not turned on for `hookOnStop()`, causing `stopRent()` reverts; In (2), the revert happens at Stop.sol - `_removeHooks()` -` try IHook(target).onStop(...) {} catch ...{revert`. In `_removeHooks()`, even though try-catch block is used, any error during the call to `onStop()` will explicitly revert the entire transaction.

```solidity
//src/policies/Stop.sol

    function stopRent(RentalOrder calldata order) external {
...
        if (order.hooks.length > 0) {
            //@audit A rentalOrder hook has to be enabled for both hookOnStart and hookOnStop, otherwise the rental cannot be stopped.
|>          _removeHooks(order.hooks, order.items, order.rentalWallet);
        }

  }

    function _removeHooks(
        Hook[] calldata hooks,
        Item[] calldata rentalItems,
        address rentalWallet
    ) internal {
...
        for (uint256 i = 0; i < hooks.length; ++i) {
...
            //@audit if a hook is only enabled for `hookOnStart()`, this will revert the `stopRent()` flow
|>          if (!STORE.hookOnStop(target)) {
                revert Errors.Shared_DisabledHook(target);
            }
...
            try
                IHook(target).onStop(
                    rentalWallet,
                    item.token,
                    item.identifier,
                    item.amount,
                    hooks[i].extraData
                )
            {} catch Error(string memory revertReason) {
                // Revert with reason given.
                revert Errors.Shared_HookFailString(revertReason);
```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L210)

As seen above, even if a hook is only intended to run on a rental creation, not on rental stop, the hook bitmap stored in `mapping(address hook => uint8 enabled) public hookStatus` has to be enabled for both `hookOnStart()` and `hookOnStop()` for it be used in a rental order. In any case if a hook is not enabled for `hookOnStop()`, a rental order cannot be stopped, and the lent assets cannot be returned to lender, and the erc20 payment will be freezed in PaymentEscrow.sol as well.

**Recommendations:**
In `_removeHooks()`, consider only try-catch call `IHook(target).onStop()` when `STORE.hookOnStop(target)` and if `!STORE.hookOnStop(target)`, simply skip the iteration instead of reverting.
                 

### Low-04: `constructor(Kernel kernel_)` has no effects when PaymentEscrow.sol is used as a logic contract.

PaymentEscrow.sol and Storage.sol are both intended be implementation contracts (not proxy contracts) because they both inherit Proxiable.sol, a contract that contains upgrade logic for implementation contracts based on UUPS standard.

In this case, storing kernel address in a logic contract will have no effect than actual flow, because kernel will be stored and fetched from the proxy contract. This can also be confirmed from test (test/integration/upgradeability/StorageModuleUpgrade.t.sol) where a new storage contract will need to be deployed with address(0) as a constructor argument. 
(1)
```solidity
//src/modules/Storage.sol
    constructor(Kernel kernel_) Module(kernel_) {}
```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L79)
(2)
```solidity
//src/modules/PaymentEscrow.sol
    constructor(Kernel kernel_) Module(kernel_) {}
```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L51)

**Recommendations:**
Consider removing the constructor.


### Low-05: The old and inactive Stop.sol policy contract cannot be disabled in the safe contract (rental wallet), even though it is no longer an active policy contract in Kernel.sol 

Stop.sol policy contract is enabled upon initialization of a rental safe contract through a delegate call to `enableModule()` and has authority to transfer asset tokens ERC721/ERC1155 out of the rental safe contract. 

However, when there is an upgrade to the stop policy,i.e. a new Stop.sol address is activated in Kernal and the old Stop.sol address is de-activated in Kernel. The old  stop.sol policy contract will remain as valid module for the rental safe contract. The rental safe contract cannot disable the old stop.sol address by calling `disableModule()`, because Guard.sol will check any transactions from the safe and will revert when `disableModule(extension)` is called and the extension is not a whitelisted extension. And because Stop.sol is never intended to be a whitelisted extension (it will be enabled at initialization by default for any safe), the safe cannot disable the old Stop.sol. 

Although under current implementation of Stop.sol, the old Stop.sol will not be able to execute `stopRent()` or `stopRentBatch()` due to the permission access checks on Storage.sol and PaymentEscrow.sol will fail and revert a call from a de-activated Stop.sol, still it's safer to disable an outdated module contract in Safe contracts.

```solidity
//src/policies/Guard.sol

    function _checkTransaction(address from, address to, bytes memory data) private view {
        bytes4 selector;
...
        } else if (selector == gnosis_safe_disable_module_selector) {
            // Load the extension address from calldata.
            address extension = address(
                uint160(
                    uint256(
                        _loadValueFromCalldata(data, gnosis_safe_disable_module_offset)
                    )
                )
            );
            // Check if the extension is whitelisted.
            //@audit the safe contract cannot disable an outdated Stop.sol, due to failing `_revertNonWhitelistedExtension(extension)`. Stop.sol doesn't have to be whitelisted and will cause revert in the disable module flow.
|>          _revertNonWhitelistedExtension(extension);

```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L266)

**Recommendations:**
Consider adding a bypass check in `_checkTransactions()` -> `else if (selector == gnosis_safe_disable_module_selector) {`, to check if the extension to be disabled is an active policy. And this can be done by querying `kernel.getPolicyIndex()` and  `kernel.activePolicies()`. 

If the extension is not an active policy, consider allowing disabling the module. Note that this logic also removes the `_revertNonWhitelistedExtension()` check and assumes that as long as a module is enabled in a safe contract, the module is either a policy (stop.sol) that is enabled by default or it used to be a whitelisted module when enabled (this is already checked in `enableModule` bypass). if this is the case, then we only need to ensure it is not an active policy to allow disable it from a safe.


### Low-06: Factory.sol might deploy a safe with old stopPolicy and guardPolicy contract addresses because it assumes stopPolicy and guardPolicy addresses will never change by storing their address as immutable.  

In Factory.sol, the stop policy contract and the guard policy contract are stored as immutable variables. This assumes these two contract addresses will never change.

```solidity
//src/policies/Factory.sol
    //@audit This assumes policy contract never changes, when policy contract change, there are risk of deploying safe with old policies and also factory has to be re-deployed to keep up with other policy contract update
|>  Stop public immutable stopPolicy;
|>  Guard public immutable guardPolicy;

```
(https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L31-L32)

However, both stop policy (Stop.sol) and guard policy (Guard.sol) can be upgraded by deploying new stop and guard contracts and deactivate the old addresses from Kernel.sol. The policy upgrade flow is: Kernel.sol - `executeAction()`->`_activatePolicy(newPolicy)`->`_deactivePolicy(oldPolicy)`.

When policy contracts are upgraded, their address changes. In this case, Factory.sol will still deploy a safe with the old policy contract addresses. This opens up potential for DOS or other vulnerabilities that the outdated policy contracts may pose to a rental safe contract. Even though the new safe contract can also migrate to the new policy contracts, the owner might not do so or is not aware because deploying safe is a permissionless process.

To avoid the risks above, the admin would have to deploy new Factory.sol every time there is upgrade to Stop.sol or Guard.sol, and also ensure that Factory upgrade is done in the same transaction as Stop.sol and Guard.sol, which is cumbersome and wasteful.

**Recommendations:**
In Factory.sol, do not use immutable variables to hold Stop.sol and Guard.sol addresses. Instead, consider adding an onlyKernel function to allow update of Stop and Guard addresses stored in Factory.sol.

