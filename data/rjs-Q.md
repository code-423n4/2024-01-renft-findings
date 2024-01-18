> Please note: this report has been reviewed to ensure it *does not* contain automated findings from the bot race winner or 4naly3er

# Summary

## Low Issues
| # | Title | Instances |
| - | - | - |
| L-01 | [`call()`/`delegatecall()` should specify a gas limit](#l-01-calldelegatecall-should-specify-a-gas-limit) | 1 |
| L-02 | [`call()`/`delegatecall()` should check for contract-existence](#l-02-calldelegatecall-should-check-for-contract-existence) | 1 |
| L-03 | [External calls in an un-bounded `for-`loop may result in a DOS](#l-03-external-calls-in-an-un-bounded-for-loop-may-result-in-a-dos) | 5 |

## Non-Critical Issues
| # | Title | Instances |
| - | - | - |
| N-01 | [Assembly blocks should have extensive comments](#n-01-assembly-blocks-should-have-extensive-comments) | 2 |
| N-02 | [Typos](#n-02-typos) | 16 |
| N-03 | [State variables should include comments](#n-03-state-variables-should-include-comments) | 22 |
| N-04 | [Duplicate strings defined](#n-04-duplicate-strings-defined) | 4 |
| N-05 | [Contract should expose an `interface`](#n-05-contract-should-expose-an-interface) | 17 |
| N-06 | [Duplicate functions defined](#n-06-duplicate-functions-defined) | 7 |
| N-07 | [Duplicate `require()`/`revert()` checks should be refactored to a modifier or function](#n-07-duplicate-requirerevert-checks-should-be-refactored-to-a-modifier-or-function) | 1 |
| N-08 | [Events are missing sender information](#n-08-events-are-missing-sender-information) | 8 |
| N-09 | [Events that mark critical parameter changes should contain both the old and the new value](#n-09-events-that-mark-critical-parameter-changes-should-contain-both-the-old-and-the-new-value) | 1 |
| N-10 | [`public` functions not called by the contract should be declared `external` instead](#n-10-public-functions-not-called-by-the-contract-should-be-declared-external-instead) | 5 |
| N-11 | [Function has excessive length](#n-11-function-has-excessive-length) | 5 |
| N-12 | [Consider moving `msg.sender` checks to `modifier`s](#n-12-consider-moving-msgsender-checks-to-modifiers) | 6 |
| N-13 | [Named imports of base contracts are missing](#n-13-named-imports-of-base-contracts-are-missing) | 4 |
| N-14 | [Complex arithmetic expression](#n-14-complex-arithmetic-expression) | 1 |
| N-15 | [Potential `block.timestamp` off-by-one error](#n-15-potential-blocktimestamp-off-by-one-error) | 1 |
| N-16 | [Naming: name immutables using all-uppercase](#n-16-naming-name-immutables-using-all-uppercase) | 6 |
| N-17 | [Naming: don't use uppercase for non-`constant` and non-`immutable` variables](#n-17-naming-dont-use-uppercase-for-non-constant-and-non-immutable-variables) | 8 |
| N-18 | [Style guide: State and local variables should be named using lowerCamelCase](#n-18-style-guide-state-and-local-variables-should-be-named-using-lowercamelcase) | 9 |
| N-19 | [Style guide: Non-`external`/`public` variable names should begin with an underscore](#n-19-style-guide-non-externalpublic-variable-names-should-begin-with-an-underscore) | 2 |
| N-20 | [NatSpec: Function `@return` is missing](#n-20-natspec-function-return-is-missing) | 51 |
| N-21 | [NatSpec: missing from file](#n-21-natspec-missing-from-file) | 12 |
| N-22 | [NatSpec: state variable declarations should have @dev tag](#n-22-natspec-state-variable-declarations-should-have-dev-tag) | 53 |
| N-23 | [NatSpec: public state variable declarations should have @notice tag](#n-23-natspec-public-state-variable-declarations-should-have-notice-tag) | 37 |
| N-24 | [Contract order does not follow Solidity style guide recommendations](#n-24-contract-order-does-not-follow-solidity-style-guide-recommendations) | 4 |
| N-25 | [Consider adding a block/deny-list](#n-25-consider-adding-a-blockdeny-list) | 1 |
| N-26 | [Contracts and libraries should use fixed compiler versions](#n-26-contracts-and-libraries-should-use-fixed-compiler-versions) | 12 |
| N-27 | [Use the latest Solidity version for deployment](#n-27-use-the-latest-solidity-version-for-deployment) | 12 |
| N-28 | [Style: surround top level declarations with two blank lines](#n-28-style-surround-top-level-declarations-with-two-blank-lines) | 12 |
| N-29 | [Large multiples of ten should use scientific notation (e.g. `1e6`) rather than decimal literals (e.g. `1000000`), for readability](#n-29-large-multiples-of-ten-should-use-scientific-notation-eg-1e6-rather-than-decimal-literals-eg-1000000-for-readability) | 2 |
| N-30 | [Consider using `delete` rather than assigning zero/false to clear values](#n-30-consider-using-delete-rather-than-assigning-zerofalse-to-clear-values) | 1 |
| N-31 | [Unnecessary cast](#n-31-unnecessary-cast) | 1 |
| N-32 | [Syntax: unnecessary `override`](#n-32-syntax-unnecessary-override) | 17 |
| N-33 | [Unused `function` definition](#n-33-unused-function-definition) | 8 |
| N-34 | [Unused import](#n-34-unused-import) | 8 |
| N-35 | [Function with unused parameter](#n-35-function-with-unused-parameter) | 1 |
| N-36 | [Named but unused function parameters in an overridden function](#n-36-named-but-unused-function-parameters-in-an-overridden-function) | 1 |

# Low Issues

## [L-01] `call()`/`delegatecall()` should specify a gas limit

There is no limit specified on the amount of gas used, so the recipient can use up all of the transaction's gas, causing it to revert. Use `addr.call{gas: <amount>}("")` or [this](https://github.com/nomad-xyz/ExcessivelySafeCall) library instead.

*Instances: 1*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Specify { gas: <amount> } option
 102 |  (bool success, bytes memory data) = token.call(
 103 |      abi.encodeWithSelector(IERC20.transfer.selector, to, value)
 104 |  );
```
GitHub: [102-104](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L102-L104)


## [L-02] `call()`/`delegatecall()` should check for contract-existence

Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`.

*Instances: 1*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Add contract existence checks
 102 |  (bool success, bytes memory data) = token.call(
 103 |      abi.encodeWithSelector(IERC20.transfer.selector, to, value)
 104 |  );
```
GitHub: [102-104](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L102-L104)


## [L-03] External calls in an un-bounded `for-`loop may result in a DOS

Consider limiting the number of iterations in `for-`loops that make external calls.

*Instances: 5*

```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-info Consider a hard bound on this loop expression value
 475 |  for (uint256 i = 0; i < hooks.length; ++i) {
  :  |
     |      // @audit-issue This external call could exhaust gas in aggregate
 480 |      if (!STORE.hookOnStart(target)) {
  :  |
     |          // @audit-issue This external call could exhaust gas in aggregate
 497 |          IHook(target).onStart(
 498 |              rentalWallet,
 499 |              offer.token,
 500 |              offer.identifier,
 501 |              offer.amount,
 502 |              hooks[i].extraData
 503 |          )

     |  // @audit-info Consider a hard bound on this loop expression value
 599 |  for (uint256 i = 0; i < items.length; ++i) {
  :  |
     |          // @audit-issue This external call could exhaust gas in aggregate
 601 |          ESCRW.increaseDeposit(items[i].token, items[i].amount);
```
GitHub: [480](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L480), [497-503](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L497-L503), [601](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L601)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-info Consider a hard bound on this loop expression value
 205 |  for (uint256 i = 0; i < hooks.length; ++i) {
  :  |
     |      // @audit-issue This external call could exhaust gas in aggregate
 210 |      if (!STORE.hookOnStop(target)) {
  :  |
     |          // @audit-issue This external call could exhaust gas in aggregate
 227 |          IHook(target).onStop(
 228 |              rentalWallet,
 229 |              item.token,
 230 |              item.identifier,
 231 |              item.amount,
 232 |              hooks[i].extraData
 233 |          )
```
GitHub: [210](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L210), [227-233](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L227-L233)

# Non-Critical Issues

## [N-01] Assembly blocks should have extensive comments

Assembly blocks take a lot more time to audit than normal Solidity code, and often have gotchas and side-effects that the Solidity versions of the same code do not. Consider adding more comments explaining what is being done in every step of the assembly code, and describe why assembly is being used instead of Solidity.

*Instances: 2*

```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Inline assembly should be heavily commented
 113 |  assembly {
 114 |      value := mload(add(data, offset))
 115 |  }

     |  // @audit-issue Inline assembly should be heavily commented
 199 |  assembly {
 200 |      selector := mload(add(data, 0x20))
 201 |  }
```
GitHub: [113-115](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L113-L115), [199-201](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L199-L201)


## [N-02] Typos

Consider correcting these spelling errors.

*Instances: 16*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Typo: renturn
  94 |  * @dev Safe transfer for ERC20 tokens that do not consistently renturn true/false.
  :  |
     |  // @audit-issue Typo: amoutn
 130 |  * @return lenderAmount Payment amoutn to send to the lender.
  :  |
     |  // @audit-issue Typo: accoutn
 155 |  * @param renter      Renter accoutn.
  :  |
     |  // @audit-issue Typo: ordesr
 335 |  * @param orders Rental ordesr for which to settle payments.
```
GitHub: [94](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L94), [130](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L130), [155](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L155), [335](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L335)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Typo: Addtionally
 208 |  *         record of the order. Addtionally, rental asset IDs are removed from
     |  // @audit-issue Typo: blocklisted
 209 |  *         storage. Once these hashes are removed, they are no longer blocklisted
  :  |
     |  // @audit-issue Typo: addresss
 269 |  * @notice Adds the addresss of a rental safe to storage so that protocol-deployed
  :  |
     |  // @audit-issue Typo: indicatingwhether
 345 |  * @param isEnabled Boolean indicatingwhether the module is enabled.
```
GitHub: [208](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L208), [209](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L209), [269](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L269), [345](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L345)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Typo: initates
  78 |  // initates the reclaim. In the context of a delegate call, address(this)
```
GitHub: [78](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L78)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Typo: fulfilmment
 207 |  // Derive and return the fulfilmment hash as specified by EIP-712
```
GitHub: [207](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L207)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Typo: implemention
 124 |  * @param newImplementation Address of the new implemention.
  :  |
     |  // @audit-issue Typo: implemention
 142 |  * @param newImplementation Address of the new implemention.
```
GitHub: [124](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L124), [142](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L142)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Typo: targetted
 192 |  * @param to Address that the data is targetted to.
```
GitHub: [192](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L192)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Typo: Wmit
 112 |  // Wmit the event.
```
GitHub: [112](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L112)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Typo: addres
  30 |  * @return deploymentAddress The addres of the deployed contract.
  :  |
     |  // @audit-issue Typo: sendder
  99 |  *         This function ties the deployment sendder to the salt of the CREATE2
```
GitHub: [30](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L30), [99](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L99)


## [N-03] State variables should include comments

Consider adding some comments on critical state variables to explain what they are supposed to do: this will help for future code reviews.

*Instances: 22*

```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Consider adding comments explaining this state var
  26 |  string internal constant _VERSION = "1.0.0";

     |  // @audit-issue Consider adding comments explaining this state var
  30 |  bytes32 internal immutable _VERSION_HASH;

     |  // @audit-issue Consider adding comments explaining this state var
  31 |  bytes32 internal immutable _EIP_712_DOMAIN_TYPEHASH;

     |  // @audit-issue Consider adding comments explaining this state var
  32 |  uint256 internal immutable _CHAIN_ID;

     |  // @audit-issue Consider adding comments explaining this state var
  33 |  bytes32 internal immutable _DOMAIN_SEPARATOR;

     |  // @audit-issue Consider adding comments explaining this state var
  34 |  bytes32 internal immutable _ITEM_TYPEHASH;

     |  // @audit-issue Consider adding comments explaining this state var
  35 |  bytes32 internal immutable _HOOK_TYPEHASH;

     |  // @audit-issue Consider adding comments explaining this state var
  36 |  bytes32 internal immutable _RENTAL_ORDER_TYPEHASH;

     |  // @audit-issue Consider adding comments explaining this state var
  37 |  bytes32 internal immutable _ORDER_FULFILLMENT_TYPEHASH;

     |  // @audit-issue Consider adding comments explaining this state var
  38 |  bytes32 internal immutable _ORDER_METADATA_TYPEHASH;

     |  // @audit-issue Consider adding comments explaining this state var
  39 |  bytes32 internal immutable _RENT_PAYLOAD_TYPEHASH;
```
GitHub: [26](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L26), [30](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L30), [31](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L31), [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L32), [33](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L33), [34](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L34), [35](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L35), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L36), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L37), [38](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L38), [39](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L39)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Consider adding comments explaining this state var
  22 |  PaymentEscrow public ESCRW;
```
GitHub: [22](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L22)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Consider adding comments explaining this state var
  54 |  PaymentEscrow public ESCRW;
```
GitHub: [54](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L54)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Consider adding comments explaining this state var
  32 |  Guard public immutable guardPolicy;

     |  // @audit-issue Consider adding comments explaining this state var
  36 |  SafeProxyFactory public immutable safeProxyFactory;

     |  // @audit-issue Consider adding comments explaining this state var
  37 |  SafeL2 public immutable safeSingleton;
```
GitHub: [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L32), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L36), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L37)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Consider adding comments explaining this state var
  45 |  PaymentEscrow public ESCRW;
```
GitHub: [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L45)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Consider adding comments explaining this state var
 209 |  address public admin;

     |  // @audit-issue Consider adding comments explaining this state var
 213 |  mapping(Keycode => Module) public getModuleForKeycode; // get contract for module keycode.

     |  // @audit-issue Consider adding comments explaining this state var
 218 |  mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

     |  // @audit-issue Consider adding comments explaining this state var
 226 |  mapping(Policy => uint256) public getPolicyIndex;

     |  // @audit-issue Consider adding comments explaining this state var
 230 |  mapping(Role => bool) public isRole;
```
GitHub: [209](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L209), [213](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L213), [218](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L218), [226](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L226), [230](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L230)


## [N-04] Duplicate strings defined

For better maintainability, consider creating and using a constant for those strings instead of duplicating it.

*Instances: 4*

```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-info Duplicates of this string were detected
 102 |  ) external onlyRole("ADMIN_ADMIN") {
  :  |
     |  // @audit-issue First seen at smart-contracts/src/policies/Admin.sol:102
 116 |  ) external onlyRole("ADMIN_ADMIN") {
  :  |
     |  // @audit-issue First seen at smart-contracts/src/policies/Admin.sol:102
 146 |  ) external onlyRole("ADMIN_ADMIN") {
```
GitHub: [116](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L116), [146](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L146)


```solidity
File: smart-contracts/src/policies/Create.sol

     |      // @audit-info Duplicates of this string were detected
 513 |      string.concat("Hook reverted: Panic code ", stringErrorCode)

File: smart-contracts/src/policies/Guard.sol
     |  // @audit-issue First seen at smart-contracts/src/policies/Create.sol:513
 178 |  string.concat("Hook reverted: Panic code ", stringErrorCode)

File: smart-contracts/src/policies/Stop.sol
     |      // @audit-issue First seen at smart-contracts/src/policies/Create.sol:513
 243 |      string.concat("Hook reverted: Panic code ", stringErrorCode)
```
GitHub: [178](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L178), [243](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L243)


## [N-05] Contract should expose an `interface`

The `contract`s should expose an `interface` so that other projects can more easily integrate with it, without having to develop their own non-standard variants.

*Instances: 17*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Consider defining an interface for this contract
  23 |  contract PaymentEscrowBase {

     |  // @audit-issue Consider defining an interface for this contract
  37 |  contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
```
GitHub: [23](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L23), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L37)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Consider defining an interface for this contract
  14 |  contract StorageBase {

     |  // @audit-issue Consider defining an interface for this contract
  66 |  contract Storage is Proxiable, Module, StorageBase {
```
GitHub: [14](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L14), [66](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L66)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Consider defining an interface for this contract
  12 |  abstract contract Accumulator {
```
GitHub: [12](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L12)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Consider defining an interface for this contract
  15 |  abstract contract Reclaimer {
```
GitHub: [15](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L15)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Consider defining an interface for this contract
  21 |  abstract contract Signer {
```
GitHub: [21](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L21)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Consider defining an interface for this contract
  15 |  contract Admin is Policy {
```
GitHub: [15](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L15)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Consider defining an interface for this contract
  41 |  contract Create is Policy, Signer, Zone, Accumulator {
```
GitHub: [41](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L41)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Consider defining an interface for this contract
  22 |  contract Factory is Policy {
```
GitHub: [22](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L22)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Consider defining an interface for this contract
  39 |  contract Guard is Policy, BaseGuard {
```
GitHub: [39](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L39)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Consider defining an interface for this contract
  34 |  contract Stop is Policy, Signer, Reclaimer, Accumulator {
```
GitHub: [34](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L34)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Consider defining an interface for this contract
  14 |  contract Create2Deployer {
```
GitHub: [14](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L14)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Consider defining an interface for this contract
  23 |  abstract contract KernelAdapter {

     |  // @audit-issue Consider defining an interface for this contract
  64 |  abstract contract Module is KernelAdapter {

     |  // @audit-issue Consider defining an interface for this contract
 114 |  abstract contract Policy is KernelAdapter {

     |  // @audit-issue Consider defining an interface for this contract
 206 |  contract Kernel {
```
GitHub: [23](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L23), [64](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L64), [114](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L114), [206](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L206)


## [N-06] Duplicate functions defined

Where possible, the following duplicated function definitions should be refactored into a common base contract or library.

*Instances: 7*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Duplicates of this function were detected
  58 |  function MODULE_PROXY_INSTANTIATION(
  59 |      Kernel kernel_
  60 |  ) external onlyByProxy onlyUninitialized {
  61 |      kernel = kernel_;
  62 |      initialized = true;
  63 |  }

File: smart-contracts/src/modules/Storage.sol
     |  // @audit-issue First seen at smart-contracts/src/modules/PaymentEscrow.sol:58
  86 |  function MODULE_PROXY_INSTANTIATION(
  87 |      Kernel kernel_
  88 |  ) external onlyByProxy onlyUninitialized {
  89 |      kernel = kernel_;
  90 |      initialized = true;
  91 |  }
```
GitHub: [86-91](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L86-L91)


```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Duplicates of this function were detected
  68 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {
  69 |      return (1, 0);
  70 |  }

File: smart-contracts/src/modules/Storage.sol
     |  // @audit-issue First seen at smart-contracts/src/modules/PaymentEscrow.sol:68
  96 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {
  97 |      return (1, 0);
  98 |  }
```
GitHub: [96-98](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L96-L98)


```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Duplicates of this function were detected
 420 |  function upgrade(address newImplementation) external onlyByProxy permissioned {
 421 |      // _upgrade is implemented in the Proxiable contract.
 422 |      _upgrade(newImplementation);
 423 |  }

File: smart-contracts/src/modules/Storage.sol
     |  // @audit-issue First seen at smart-contracts/src/modules/PaymentEscrow.sol:420
 360 |  function upgrade(address newImplementation) external onlyByProxy permissioned {
 361 |      // _upgrade is implemented in the Proxiable contract.
 362 |      _upgrade(newImplementation);
 363 |  }
```
GitHub: [360-363](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L360-L363)


```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Duplicates of this function were detected
 429 |  function freeze() external onlyByProxy permissioned {
 430 |      // _freeze is implemented in the Proxiable contract.
 431 |      _freeze();
 432 |  }

File: smart-contracts/src/modules/Storage.sol
     |  // @audit-issue First seen at smart-contracts/src/modules/PaymentEscrow.sol:429
 369 |  function freeze() external onlyByProxy permissioned {
 370 |      // _freeze is implemented in the Proxiable contract.
 371 |      _freeze();
 372 |  }
```
GitHub: [369-372](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L369-L372)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-info Duplicates of this function were detected
  40 |      function configureDependencies()
  41 |          external
  42 |          override
  43 |          onlyKernel
  44 |          returns (Keycode[] memory dependencies)
  45 |      {
  46 |          dependencies = new Keycode[](2);
  47 |  
  48 |          dependencies[0] = toKeycode("STORE");
  49 |          STORE = Storage(getModuleAddress(toKeycode("STORE")));
  50 |  
  51 |          dependencies[1] = toKeycode("ESCRW");
  52 |          ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
  53 |      }

File: smart-contracts/src/policies/Create.sol
     |  // @audit-issue First seen at smart-contracts/src/policies/Admin.sol:40
  72 |      function configureDependencies()
  73 |          external
  74 |          override
  75 |          onlyKernel
  76 |          returns (Keycode[] memory dependencies)
  77 |      {
  78 |          dependencies = new Keycode[](2);
  79 |  
  80 |          dependencies[0] = toKeycode("STORE");
  81 |          STORE = Storage(getModuleAddress(toKeycode("STORE")));
  82 |  
  83 |          dependencies[1] = toKeycode("ESCRW");
  84 |          ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
  85 |      }

File: smart-contracts/src/policies/Stop.sol
     |  // @audit-issue First seen at smart-contracts/src/policies/Admin.sol:40
  63 |      function configureDependencies()
  64 |          external
  65 |          override
  66 |          onlyKernel
  67 |          returns (Keycode[] memory dependencies)
  68 |      {
  69 |          dependencies = new Keycode[](2);
  70 |  
  71 |          dependencies[0] = toKeycode("STORE");
  72 |          STORE = Storage(getModuleAddress(toKeycode("STORE")));
  73 |  
  74 |          dependencies[1] = toKeycode("ESCRW");
  75 |          ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")));
  76 |      }
```
GitHub: [72-85](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L72-L85), [63-76](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L63-L76)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-info Duplicates of this function were detected
  73 |      function configureDependencies()
  74 |          external
  75 |          override
  76 |          onlyKernel
  77 |          returns (Keycode[] memory dependencies)
  78 |      {
  79 |          dependencies = new Keycode[](1);
  80 |  
  81 |          dependencies[0] = toKeycode("STORE");
  82 |          STORE = Storage(getModuleAddress(toKeycode("STORE")));
  83 |      }

File: smart-contracts/src/policies/Guard.sol
     |  // @audit-issue First seen at smart-contracts/src/policies/Factory.sol:73
  63 |      function configureDependencies()
  64 |          external
  65 |          override
  66 |          onlyKernel
  67 |          returns (Keycode[] memory dependencies)
  68 |      {
  69 |          dependencies = new Keycode[](1);
  70 |  
  71 |          dependencies[0] = toKeycode("STORE");
  72 |          STORE = Storage(getModuleAddress(toKeycode("STORE")));
  73 |      }
```
GitHub: [63-73](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L63-L73)


## [N-07] Duplicate `require()`/`revert()` checks should be refactored to a modifier or function

The compiler will inline the function, which will avoid `JUMP` instructions usually associated with functions.

*Instances: 1*

```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info Duplicates of this conditional revert statement were detected
 299 |  if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);
  :  |
     |  // @audit-issue First seen at smart-contracts/src/modules/Storage.sol:299
 318 |  if (hook.code.length == 0) revert Errors.StorageModule_NotContract(hook);
```
GitHub: [318](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L318)


## [N-08] Events are missing sender information

When an action is triggered based on a user's action, not being able to filter based on who triggered the action makes event processing a lot more cumbersome. Including the `msg.sender` the events of these types of action will make events much more useful to end users, especially when `msg.sender` is not `tx.origin`.

*Instances: 8*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Consider adding msg.sender as a parameter
 411 |  emit Events.FeeTaken(token, skimmedBalance);
```
GitHub: [411](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L411)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Consider adding msg.sender as a parameter
 171 |  emit Events.RentalOrderStarted(
 172 |      orderHash,
 173 |      extraData,
 174 |      order.seaportOrderHash,
 175 |      order.items,
 176 |      order.hooks,
 177 |      order.orderType,
 178 |      order.lender,
 179 |      order.renter,
 180 |      order.rentalWallet,
 181 |      order.startTimestamp,
 182 |      order.endTimestamp
 183 |  );
```
GitHub: [171-183](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L171-L183)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Consider adding msg.sender as a parameter
 192 |  emit Events.RentalSafeDeployment(safe, owners, threshold);
```
GitHub: [192](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L192)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Consider adding msg.sender as a parameter
 113 |  emit Events.RentalOrderStopped(seaportOrderHash, stopper);
```
GitHub: [113](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L113)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Consider adding msg.sender as a parameter
 301 |  emit Events.ActionExecuted(action_, target_);

     |  // @audit-issue Consider adding msg.sender as a parameter
 324 |  emit Events.RoleGranted(role_, addr_);

     |  // @audit-issue Consider adding msg.sender as a parameter
 344 |  emit Events.RoleRevoked(role_, addr_);

     |  // @audit-issue Consider adding msg.sender as a parameter
 571 |  emit Events.PermissionsUpdated(
 572 |      request.keycode,
 573 |      policy_,
 574 |      request.funcSelector,
 575 |      grant_
 576 |  );
```
GitHub: [301](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L301), [324](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L324), [344](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L344), [571-576](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L571-L576)


## [N-09] Events that mark critical parameter changes should contain both the old and the new value

This should especially be done if the new value is not required to be different from the old value

*Instances: 1*

```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info setter/update
 560 |  function _setPolicyPermissions(
 561 |      Policy policy_,
 562 |      Permissions[] memory requests_,
 563 |      bool grant_
 564 |  ) internal {
  :  |
     |          // @audit-issue Consider including 'old' parameters in emit
 571 |          emit Events.PermissionsUpdated(
 572 |              request.keycode,
 573 |              policy_,
 574 |              request.funcSelector,
 575 |              grant_
 576 |          );
```
GitHub: [571-576](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L571-L576)


## [N-10] `public` functions not called by the contract should be declared `external` instead

Contracts [are allowed](https://docs.soliditylang.org/en/latest/contracts.html#function-overriding) to override their parents' functions and change the visibility from `external` to `public`.

*Instances: 5*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-info Context
  37 |  contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
  :  |
     |      // @audit-issue Not called internally by this contract
  75 |      function KEYCODE() public pure override returns (Keycode) {
```
GitHub: [75](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L75)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-info Context
  66 |  contract Storage is Proxiable, Module, StorageBase {
  :  |
     |      // @audit-issue Not called internally by this contract
 103 |      function KEYCODE() public pure override returns (Keycode) {
```
GitHub: [103](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L103)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-info Context
  14 |  contract Create2Deployer {
  :  |
     |      // @audit-issue Not called internally by this contract
 107 |      function generateSaltWithSender(
 108 |          address sender,
 109 |          bytes12 data
 110 |      ) public pure returns (bytes32 salt) {
```
GitHub: [107-110](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L107-L110)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info Context
 206 |  contract Kernel {
  :  |
     |      // @audit-issue Not called internally by this contract
 310 |      function grantRole(Role role_, address addr_) public onlyAdmin {
  :  |
     |      // @audit-issue Not called internally by this contract
 333 |      function revokeRole(Role role_, address addr_) public onlyAdmin {
```
GitHub: [310](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L310), [333](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L333)


## [N-11] Function has excessive length

Long functions are more difficult to understand and test, and ought to be refactored where possible to reduce surprises.

*Instances: 5*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Long function: 69 lines
 215 |  function _settlePayment(
 216 |      Item[] calldata items,
 217 |      OrderType orderType,
 218 |      address lender,
 219 |      address renter,
 220 |      uint256 start,
 221 |      uint256 end
 222 |  ) internal {
```
GitHub: [215-222](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L215-L222)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Long function: 70 lines
 339 |  function _deriveRentalTypehashes()
 340 |      internal
 341 |      pure
 342 |      returns (
 343 |          bytes32 itemTypeHash,
 344 |          bytes32 hookTypeHash,
 345 |          bytes32 rentalOrderTypeHash,
 346 |          bytes32 orderFulfillmentTypeHash,
 347 |          bytes32 orderMetadataTypeHash,
 348 |          bytes32 rentPayloadTypeHash
 349 |      )
 350 |  {
```
GitHub: [339-350](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L339-L350)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Long function: 68 lines
 247 |  function _processPayOrderOffer(
 248 |      Item[] memory rentalItems,
 249 |      SpentItem[] memory offers,
 250 |      uint256 startIndex
 251 |  ) internal pure {

     |  // @audit-issue Long function: 88 lines
 530 |  function _rentFromZone(
 531 |      RentPayload memory payload,
 532 |      SeaportPayload memory seaportPayload
 533 |  ) internal {
```
GitHub: [247-251](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L247-L251), [530-533](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L530-L533)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Long function: 99 lines
 195 |  function _checkTransaction(address from, address to, bytes memory data) private view {
```
GitHub: [195](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195)


## [N-12] Consider moving `msg.sender` checks to `modifier`s

Using `modifier`s makes the contract guard intention of the code more clear, and also guarantees no other code can run before the modifier runs.

*Instances: 6*

```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Refactor into a modifier
  37 |  if (address(bytes20(salt)) != msg.sender) {
  38 |      revert Errors.Create2Deployer_UnauthorizedSender(msg.sender, salt);
  39 |  }
```
GitHub: [37-39](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L37-L39)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Refactor into a modifier
  41 |  if (msg.sender != address(kernel))
  42 |      revert Errors.KernelAdapter_OnlyKernel(msg.sender);

     |  // @audit-issue Refactor into a modifier
  78 |  if (!kernel.modulePermissions(KEYCODE(), Policy(msg.sender), msg.sig)) {
  79 |      revert Errors.Module_PolicyNotAuthorized(msg.sender);
  80 |  }

     |  // @audit-issue Refactor into a modifier
 132 |  if (!kernel.hasRole(msg.sender, role)) {
 133 |      revert Errors.Policy_OnlyRole(role);
 134 |  }

     |  // @audit-issue Refactor into a modifier
 255 |  if (msg.sender != executor) revert Errors.Kernel_OnlyExecutor(msg.sender);

     |  // @audit-issue Refactor into a modifier
 263 |  if (msg.sender != admin) revert Errors.Kernel_OnlyAdmin(msg.sender);
```
GitHub: [41-42](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L41-L42), [78-80](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L78-L80), [132-134](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L132-L134), [255](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L255), [263](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L263)


## [N-13] Named imports of base contracts are missing

When importing base contracts, use [named imports](https://docs.soliditylang.org/en/latest/layout-of-source-files.html#syntax-and-semantics) to improve code readability.

*Instances: 4*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Missing named import for `PaymentEscrowBase`
  37 |  contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
```
GitHub: [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L37)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Missing named import for `StorageBase`
  66 |  contract Storage is Proxiable, Module, StorageBase {
```
GitHub: [66](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L66)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Missing named import for `KernelAdapter`
  64 |  abstract contract Module is KernelAdapter {

     |  // @audit-issue Missing named import for `KernelAdapter`
 114 |  abstract contract Policy is KernelAdapter {
```
GitHub: [64](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L64), [114](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L114)


## [N-14] Complex arithmetic expression

To maintain readability in code, particularly in Solidity which can involve complex mathematical operations, it is often recommended to limit the number of arithmetic operations to a maximum of 2-3 per line. Too many operations in a single line can make the code difficult to read and understand, increase the likelihood of mistakes, and complicate the process of debugging and reviewing the code. Consider splitting such operations over more than one line, take special care when dealing with division however. Try to limit the number of arithmetic operations to a maximum of 3 per line.

*Instances: 1*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Use unchecked to save gas, if possible
 142 |  renterAmount = ((numerator / totalTime) + 500) / 1000;
```
GitHub: [142](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L142)


## [N-15] Potential `block.timestamp` off-by-one error

In Solidity, using `>=` or `<=` to compare against `block.timestamp` (alias `now`) may introduce off-by-one errors due to the fact that `block.timestamp` is only updated once per block and its value remains constant throughout the block's execution. If an operation happens at the exact second when `block.timestamp` changes, it could result in unexpected behavior. To avoid this, it's safer to use strict inequality operators (`>` or `<`). For instance, if a condition should only be met after a certain time, use `block.timestamp > time` rather than `block.timestamp >= time`. This way, potential off-by-one errors due to the exact timing of block mining are mitigated, leading to safer, more predictable contract behavior.

*Instances: 1*

```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Consider strict inequality (< or >)
 132 |  bool hasExpired = endTimestamp <= block.timestamp;
```
GitHub: [132](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L132)


## [N-16] Naming: name immutables using all-uppercase

Immutables should be in uppercase as stated [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html#constants).

*Instances: 6*

```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Names of immutable variables should be in all uppercase
  17 |  address private immutable original;
```
GitHub: [17](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L17)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Names of immutable variables should be in all uppercase
  31 |  Stop public immutable stopPolicy;

     |  // @audit-issue Names of immutable variables should be in all uppercase
  32 |  Guard public immutable guardPolicy;

     |  // @audit-issue Names of immutable variables should be in all uppercase
  35 |  TokenCallbackHandler public immutable fallbackHandler;

     |  // @audit-issue Names of immutable variables should be in all uppercase
  36 |  SafeProxyFactory public immutable safeProxyFactory;

     |  // @audit-issue Names of immutable variables should be in all uppercase
  37 |  SafeL2 public immutable safeSingleton;
```
GitHub: [31](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L31), [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L32), [35](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L35), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L36), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L37)


## [N-17] Naming: don't use uppercase for non-`constant` and non-`immutable` variables

Variables which are not constants or immutable should **not** be declared in all uppercase.

*Instances: 8*

```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  21 |  Storage public STORE;

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  22 |  PaymentEscrow public ESCRW;
```
GitHub: [21](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L21), [22](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L22)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  53 |  Storage public STORE;

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  54 |  PaymentEscrow public ESCRW;
```
GitHub: [53](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L53), [54](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L54)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  28 |  Storage public STORE;
```
GitHub: [28](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L28)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  45 |  Storage public STORE;
```
GitHub: [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L45)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  44 |  Storage public STORE;

     |  // @audit-issue Names of non-constant/non-immutable variables should not be all uppercase
  45 |  PaymentEscrow public ESCRW;
```
GitHub: [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L44), [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L45)


## [N-18] Style guide: State and local variables should be named using lowerCamelCase

The Solidity style guide [says](https://docs.soliditylang.org/en/latest/style-guide.html#local-and-state-variable-names) to use mixedCase for local and state variable names. Note that while OpenZeppelin may not follow this advice, it still is the recommended way of naming variables.

*Instances: 9*

```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Use lowerCamelCase style in name `_rentalId`
  38 |  bytes32 _rentalId = RentalId.unwrap(rentalId);
```
GitHub: [38](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L38)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Use lowerCamelCase style in name `STORE`
  21 |  Storage public STORE;

     |  // @audit-issue Use lowerCamelCase style in name `ESCRW`
  22 |  PaymentEscrow public ESCRW;
```
GitHub: [21](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L21), [22](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L22)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Use lowerCamelCase style in name `STORE`
  53 |  Storage public STORE;

     |  // @audit-issue Use lowerCamelCase style in name `ESCRW`
  54 |  PaymentEscrow public ESCRW;
```
GitHub: [53](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L53), [54](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L54)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Use lowerCamelCase style in name `STORE`
  28 |  Storage public STORE;
```
GitHub: [28](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L28)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Use lowerCamelCase style in name `STORE`
  45 |  Storage public STORE;
```
GitHub: [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L45)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Use lowerCamelCase style in name `STORE`
  44 |  Storage public STORE;

     |  // @audit-issue Use lowerCamelCase style in name `ESCRW`
  45 |  PaymentEscrow public ESCRW;
```
GitHub: [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L44), [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L45)


## [N-19] Style guide: Non-`external`/`public` variable names should begin with an underscore

According to the Solidity Style Guide, Non-`external`/`public` variable and function names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables).

*Instances: 2*

```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Prefix variable name with an `_` (underscore)
  17 |  address private immutable original;
```
GitHub: [17](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L17)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Prefix variable name with an `_` (underscore)
  19 |  bytes1 constant create2_ff = 0xff;
```
GitHub: [19](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L19)


## [N-20] NatSpec: Function `@return` is missing

Documentation of all function parameters improves code readability.

*Instances: 51*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode 
  75 |  function KEYCODE() public pure override returns (Keycode) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 uint256 
  88 |  function _calculateFee(uint256 amount) internal view returns (uint256) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 uint256 renterAmount
 136 |  ) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {

     |  // @audit-issue Missing NatSpec @return for parameter 2 uint256 lenderAmount
 136 |  ) internal pure returns (uint256 renterAmount, uint256 lenderAmount) {
```
GitHub: [75](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L75), [88](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88), [136](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L136), [136](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L136)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode 
 103 |  function KEYCODE() public pure override returns (Keycode) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bool 
 122 |  ) external view returns (bool) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 address 
 135 |  function contractToHook(address to) external view returns (address) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bool 
 149 |  function hookOnTransaction(address hook) external view returns (bool) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bool 
 159 |  function hookOnStart(address hook) external view returns (bool) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bool 
 169 |  function hookOnStop(address hook) external view returns (bool) {
```
GitHub: [103](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L103), [122](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L122), [135](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L135), [149](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L149), [159](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L159), [169](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L169)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 struct RentalAssetUpdate[] updates
  98 |  ) internal pure returns (RentalAssetUpdate[] memory updates) {
```
GitHub: [98](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L98)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 address 
 110 |  ) internal view returns (address) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 125 |  function _deriveItemHash(Item memory item) internal view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 147 |  function _deriveHookHash(Hook memory hook) internal view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 164 |  ) internal view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 206 |  ) internal view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 220 |  ) internal view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 250 |  ) internal view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 277 |  ) internal view virtual returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 nameHash
 302 |  bytes32 nameHash,

     |  // @audit-issue Missing NatSpec @return for parameter 2 bytes32 versionHash
 303 |  bytes32 versionHash,

     |  // @audit-issue Missing NatSpec @return for parameter 3 bytes32 eip712DomainTypehash
 304 |  bytes32 eip712DomainTypehash,

     |  // @audit-issue Missing NatSpec @return for parameter 4 bytes32 domainSeparator
 305 |  bytes32 domainSeparator

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 itemTypeHash
 343 |  bytes32 itemTypeHash,

     |  // @audit-issue Missing NatSpec @return for parameter 2 bytes32 hookTypeHash
 344 |  bytes32 hookTypeHash,

     |  // @audit-issue Missing NatSpec @return for parameter 3 bytes32 rentalOrderTypeHash
 345 |  bytes32 rentalOrderTypeHash,

     |  // @audit-issue Missing NatSpec @return for parameter 4 bytes32 orderFulfillmentTypeHash
 346 |  bytes32 orderFulfillmentTypeHash,

     |  // @audit-issue Missing NatSpec @return for parameter 5 bytes32 orderMetadataTypeHash
 347 |  bytes32 orderMetadataTypeHash,

     |  // @audit-issue Missing NatSpec @return for parameter 6 bytes32 rentPayloadTypeHash
 348 |  bytes32 rentPayloadTypeHash
```
GitHub: [110](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L110), [125](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L125), [147](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L147), [164](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L164), [206](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L206), [220](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L220), [250](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L250), [277](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L277), [302](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L302), [303](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L303), [304](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L304), [305](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L305), [343](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L343), [344](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L344), [345](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L345), [346](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L346), [347](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L347), [348](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L348)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode[] dependencies
  44 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue Missing NatSpec @return for parameter 1 struct Permissions[] requests
  69 |  returns (Permissions[] memory requests)
```
GitHub: [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L44), [69](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L69)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode[] dependencies
  76 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue Missing NatSpec @return for parameter 1 struct Permissions[] requests
 101 |  returns (Permissions[] memory requests)

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 117 |  function domainSeparator() external view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 128 |  ) external view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 139 |  ) external view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 
 150 |  ) external view returns (bytes32) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes4 validOrderMagicValue
 735 |  ) external override onlyRole("SEAPORT") returns (bytes4 validOrderMagicValue) {
```
GitHub: [76](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L76), [101](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L101), [117](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L117), [128](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L128), [139](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L139), [150](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L150), [735](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L735)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode[] dependencies
  77 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue Missing NatSpec @return for parameter 1 struct Permissions[] requests
  99 |  returns (Permissions[] memory requests)
```
GitHub: [77](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L77), [99](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L99)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode[] dependencies
  67 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue Missing NatSpec @return for parameter 1 struct Permissions[] requests
  89 |  returns (Permissions[] memory requests)

     |  // @audit-issue Missing NatSpec @return for parameter 1 bytes32 value
 111 |  ) private pure returns (bytes32 value) {
```
GitHub: [67](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L67), [89](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L89), [111](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L111)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode[] dependencies
  67 |  returns (Keycode[] memory dependencies)

     |  // @audit-issue Missing NatSpec @return for parameter 1 struct Permissions[] requests
  92 |  returns (Permissions[] memory requests)
```
GitHub: [67](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L67), [92](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L92)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 address deploymentAddress
  35 |  ) external payable returns (address deploymentAddress) {

     |  // @audit-issue Missing NatSpec @return for parameter 1 address 
  87 |  ) public view returns (address) {
```
GitHub: [35](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L35), [87](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L87)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Missing NatSpec @return for parameter 1 Keycode 
  90 |  function KEYCODE() public pure virtual returns (Keycode);

     |  // @audit-issue Missing NatSpec @return for parameter 1 uint8 major
 100 |  function VERSION() external pure virtual returns (uint8 major, uint8 minor) {}

     |  // @audit-issue Missing NatSpec @return for parameter 2 uint8 minor
 100 |  function VERSION() external pure virtual returns (uint8 major, uint8 minor) {}

     |  // @audit-issue Missing NatSpec @return for parameter 1 address 
 180 |  function getModuleAddress(Keycode keycode_) internal view returns (address) {
```
GitHub: [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L90), [100](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L100), [100](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L100), [180](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L180)


## [N-21] NatSpec: missing from file

As per the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.22/style-guide.html#natspec):

        >  They are written with a triple slash (///) or a double asterisk block (/** ... */) and they should be used directly above function declarations or statements.

        Files with no NatSpec documentation are more difficult to use and review.
        

*Instances: 12*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L1)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L1)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L1)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L1)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L1)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L1)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L1)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L1)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L1)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L1)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L1)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Add NatSpec comments to this file
   1 |  // SPDX-License-Identifier: BUSL-1.1
```
GitHub: [1](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L1)


## [N-22] NatSpec: state variable declarations should have @dev tag

As per [Solidity NatSpec](https://docs.soliditylang.org/en/latest/natspec-format.html#tags). Note: public and non-public state variables should have a @dev tag. Only public state variables should have a @notice tag.

*Instances: 53*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Add NatSpec documentation
  25 |  mapping(address token => uint256 amount) public balanceOf;

     |  // @audit-issue Add NatSpec documentation
  28 |  uint256 public fee;
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L25), [28](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L28)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Add NatSpec documentation
  20 |  mapping(bytes32 orderHash => bool isActive) public orders;

     |  // @audit-issue Add NatSpec documentation
  26 |  mapping(RentalId itemId => uint256 amount) public rentedAssets;

     |  // @audit-issue Add NatSpec documentation
  33 |  mapping(address safe => uint256 nonce) public deployedSafes;

     |  // @audit-issue Add NatSpec documentation
  36 |  uint256 public totalSafes;

     |  // @audit-issue Add NatSpec documentation
  44 |  mapping(address to => address hook) internal _contractToHook;

     |  // @audit-issue Add NatSpec documentation
  47 |  mapping(address hook => uint8 enabled) public hookStatus;

     |  // @audit-issue Add NatSpec documentation
  55 |  mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;

     |  // @audit-issue Add NatSpec documentation
  58 |  mapping(address extension => bool isWhitelisted) public whitelistedExtensions;
```
GitHub: [20](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L20), [26](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L26), [33](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L33), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L36), [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L44), [47](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L47), [55](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L55), [58](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L58)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Add NatSpec documentation
  17 |  address private immutable original;
```
GitHub: [17](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L17)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Add NatSpec documentation
  25 |  string internal constant _NAME = "ReNFT-Rentals";

     |  // @audit-issue Add NatSpec documentation
  26 |  string internal constant _VERSION = "1.0.0";

     |  // @audit-issue Add NatSpec documentation
  29 |  bytes32 internal immutable _NAME_HASH;

     |  // @audit-issue Add NatSpec documentation
  30 |  bytes32 internal immutable _VERSION_HASH;

     |  // @audit-issue Add NatSpec documentation
  31 |  bytes32 internal immutable _EIP_712_DOMAIN_TYPEHASH;

     |  // @audit-issue Add NatSpec documentation
  32 |  uint256 internal immutable _CHAIN_ID;

     |  // @audit-issue Add NatSpec documentation
  33 |  bytes32 internal immutable _DOMAIN_SEPARATOR;

     |  // @audit-issue Add NatSpec documentation
  34 |  bytes32 internal immutable _ITEM_TYPEHASH;

     |  // @audit-issue Add NatSpec documentation
  35 |  bytes32 internal immutable _HOOK_TYPEHASH;

     |  // @audit-issue Add NatSpec documentation
  36 |  bytes32 internal immutable _RENTAL_ORDER_TYPEHASH;

     |  // @audit-issue Add NatSpec documentation
  37 |  bytes32 internal immutable _ORDER_FULFILLMENT_TYPEHASH;

     |  // @audit-issue Add NatSpec documentation
  38 |  bytes32 internal immutable _ORDER_METADATA_TYPEHASH;

     |  // @audit-issue Add NatSpec documentation
  39 |  bytes32 internal immutable _RENT_PAYLOAD_TYPEHASH;
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L25), [26](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L26), [29](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L29), [30](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L30), [31](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L31), [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L32), [33](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L33), [34](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L34), [35](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L35), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L36), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L37), [38](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L38), [39](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L39)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Add NatSpec documentation
  21 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  22 |  PaymentEscrow public ESCRW;
```
GitHub: [21](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L21), [22](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L22)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Add NatSpec documentation
  53 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  54 |  PaymentEscrow public ESCRW;
```
GitHub: [53](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L53), [54](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L54)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Add NatSpec documentation
  28 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  31 |  Stop public immutable stopPolicy;

     |  // @audit-issue Add NatSpec documentation
  32 |  Guard public immutable guardPolicy;

     |  // @audit-issue Add NatSpec documentation
  35 |  TokenCallbackHandler public immutable fallbackHandler;

     |  // @audit-issue Add NatSpec documentation
  36 |  SafeProxyFactory public immutable safeProxyFactory;

     |  // @audit-issue Add NatSpec documentation
  37 |  SafeL2 public immutable safeSingleton;
```
GitHub: [28](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L28), [31](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L31), [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L32), [35](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L35), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L36), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L37)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Add NatSpec documentation
  45 |  Storage public STORE;
```
GitHub: [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L45)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Add NatSpec documentation
  44 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  45 |  PaymentEscrow public ESCRW;
```
GitHub: [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L44), [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L45)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Add NatSpec documentation
  16 |  mapping(address => bool) public deployed;

     |  // @audit-issue Add NatSpec documentation
  19 |  bytes1 constant create2_ff = 0xff;
```
GitHub: [16](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L16), [19](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L19)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Add NatSpec documentation
  25 |  Kernel public kernel;

     |  // @audit-issue Add NatSpec documentation
 116 |  bool public isActive;

     |  // @audit-issue Add NatSpec documentation
 208 |  address public executor;

     |  // @audit-issue Add NatSpec documentation
 209 |  address public admin;

     |  // @audit-issue Add NatSpec documentation
 212 |  Keycode[] public allKeycodes;

     |  // @audit-issue Add NatSpec documentation
 213 |  mapping(Keycode => Module) public getModuleForKeycode; // get contract for module keycode.

     |  // @audit-issue Add NatSpec documentation
 214 |  mapping(Module => Keycode) public getKeycodeForModule; // get module keycode for contract.

     |  // @audit-issue Add NatSpec documentation
 217 |  mapping(Keycode => Policy[]) public moduleDependents;

     |  // @audit-issue Add NatSpec documentation
 218 |  mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

     |  // @audit-issue Add NatSpec documentation
 221 |  mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
 222 |      public modulePermissions; // for policy addr, check if they have permission to call the function in the module.

     |  // @audit-issue Add NatSpec documentation
 225 |  Policy[] public activePolicies;

     |  // @audit-issue Add NatSpec documentation
 226 |  mapping(Policy => uint256) public getPolicyIndex;

     |  // @audit-issue Add NatSpec documentation
 229 |  mapping(address => mapping(Role => bool)) public hasRole;

     |  // @audit-issue Add NatSpec documentation
 230 |  mapping(Role => bool) public isRole;
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L25), [116](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L116), [208](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L208), [209](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L209), [212](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L212), [213](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L213), [214](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L214), [217](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L217), [218](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L218), [221-222](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L221-L222), [225](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L225), [226](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L226), [229](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L229), [230](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L230)


## [N-23] NatSpec: public state variable declarations should have @notice tag

As per [Solidity NatSpec](https://docs.soliditylang.org/en/latest/natspec-format.html#tags).

*Instances: 37*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Add NatSpec documentation
  25 |  mapping(address token => uint256 amount) public balanceOf;

     |  // @audit-issue Add NatSpec documentation
  28 |  uint256 public fee;
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L25), [28](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L28)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Add NatSpec documentation
  20 |  mapping(bytes32 orderHash => bool isActive) public orders;

     |  // @audit-issue Add NatSpec documentation
  26 |  mapping(RentalId itemId => uint256 amount) public rentedAssets;

     |  // @audit-issue Add NatSpec documentation
  33 |  mapping(address safe => uint256 nonce) public deployedSafes;

     |  // @audit-issue Add NatSpec documentation
  36 |  uint256 public totalSafes;

     |  // @audit-issue Add NatSpec documentation
  47 |  mapping(address hook => uint8 enabled) public hookStatus;

     |  // @audit-issue Add NatSpec documentation
  55 |  mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;

     |  // @audit-issue Add NatSpec documentation
  58 |  mapping(address extension => bool isWhitelisted) public whitelistedExtensions;
```
GitHub: [20](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L20), [26](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L26), [33](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L33), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L36), [47](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L47), [55](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L55), [58](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L58)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Add NatSpec documentation
  21 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  22 |  PaymentEscrow public ESCRW;
```
GitHub: [21](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L21), [22](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L22)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Add NatSpec documentation
  53 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  54 |  PaymentEscrow public ESCRW;
```
GitHub: [53](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L53), [54](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L54)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Add NatSpec documentation
  28 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  31 |  Stop public immutable stopPolicy;

     |  // @audit-issue Add NatSpec documentation
  32 |  Guard public immutable guardPolicy;

     |  // @audit-issue Add NatSpec documentation
  35 |  TokenCallbackHandler public immutable fallbackHandler;

     |  // @audit-issue Add NatSpec documentation
  36 |  SafeProxyFactory public immutable safeProxyFactory;

     |  // @audit-issue Add NatSpec documentation
  37 |  SafeL2 public immutable safeSingleton;
```
GitHub: [28](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L28), [31](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L31), [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L32), [35](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L35), [36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L36), [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L37)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Add NatSpec documentation
  45 |  Storage public STORE;
```
GitHub: [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L45)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Add NatSpec documentation
  44 |  Storage public STORE;

     |  // @audit-issue Add NatSpec documentation
  45 |  PaymentEscrow public ESCRW;
```
GitHub: [44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L44), [45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L45)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Add NatSpec documentation
  16 |  mapping(address => bool) public deployed;
```
GitHub: [16](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L16)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Add NatSpec documentation
  25 |  Kernel public kernel;

     |  // @audit-issue Add NatSpec documentation
 116 |  bool public isActive;

     |  // @audit-issue Add NatSpec documentation
 208 |  address public executor;

     |  // @audit-issue Add NatSpec documentation
 209 |  address public admin;

     |  // @audit-issue Add NatSpec documentation
 212 |  Keycode[] public allKeycodes;

     |  // @audit-issue Add NatSpec documentation
 213 |  mapping(Keycode => Module) public getModuleForKeycode; // get contract for module keycode.

     |  // @audit-issue Add NatSpec documentation
 214 |  mapping(Module => Keycode) public getKeycodeForModule; // get module keycode for contract.

     |  // @audit-issue Add NatSpec documentation
 217 |  mapping(Keycode => Policy[]) public moduleDependents;

     |  // @audit-issue Add NatSpec documentation
 218 |  mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

     |  // @audit-issue Add NatSpec documentation
 221 |  mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
 222 |      public modulePermissions; // for policy addr, check if they have permission to call the function in the module.

     |  // @audit-issue Add NatSpec documentation
 225 |  Policy[] public activePolicies;

     |  // @audit-issue Add NatSpec documentation
 226 |  mapping(Policy => uint256) public getPolicyIndex;

     |  // @audit-issue Add NatSpec documentation
 229 |  mapping(address => mapping(Role => bool)) public hasRole;

     |  // @audit-issue Add NatSpec documentation
 230 |  mapping(Role => bool) public isRole;
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L25), [116](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L116), [208](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L208), [209](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L209), [212](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L212), [213](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L213), [214](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L214), [217](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L217), [218](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L218), [221-222](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L221-L222), [225](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L225), [226](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L226), [229](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L229), [230](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L230)


## [N-24] Contract order does not follow Solidity style guide recommendations

This is a [best practice](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-layout) that should be followed.

Inside each contract, library or interface, use the following order:

> 1. Type declarations
> 2. State variables
> 3. Events
> 4. Errors
> 5. Modifiers
> 6. Functions

Note that the recommendations here only cover the top-level ordering. Within, e.g. variables and functions, additional ordering rules will apply, but are covered by other findings.

*Instances: 4*

```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-info Contract component order has style violations; recommended top-level order:
     |  // variable kernel
     |  // modifier onlyKernel
     |  // constructor
     |  // function changeKernel
  23 |  abstract contract KernelAdapter {
  :  |
     |      // @audit-issue modifier onlyKernel should come before constructor
  40 |      modifier onlyKernel() {
  41 |          if (msg.sender != address(kernel))
  42 |              revert Errors.KernelAdapter_OnlyKernel(msg.sender);
  43 |          _;
  44 |      }

     |  // @audit-info Contract component order has style violations; recommended top-level order:
     |  // modifier permissioned
     |  // constructor
     |  // function KEYCODE
     |  // function VERSION
     |  // function INIT
  64 |  abstract contract Module is KernelAdapter {
  :  |
     |      // @audit-issue modifier permissioned should come before constructor
  77 |      modifier permissioned() {
  78 |          if (!kernel.modulePermissions(KEYCODE(), Policy(msg.sender), msg.sig)) {
  79 |              revert Errors.Module_PolicyNotAuthorized(msg.sender);
  80 |          }
  81 |          _;
  82 |      }

     |  // @audit-info Contract component order has style violations; recommended top-level order:
     |  // variable isActive
     |  // modifier onlyRole
     |  // constructor
     |  // function configureDependencies
     |  // function requestPermissions
     |  // function getModuleAddress
     |  // function setActiveStatus
 114 |  abstract contract Policy is KernelAdapter {
  :  |
     |      // @audit-issue modifier onlyRole should come before constructor
 130 |      modifier onlyRole(bytes32 role_) {
 131 |          Role role = toRole(role_);
 132 |          if (!kernel.hasRole(msg.sender, role)) {
 133 |              revert Errors.Policy_OnlyRole(role);
 134 |          }
 135 |          _;
 136 |      }

     |  // @audit-info Contract component order has style violations; recommended top-level order:
     |  // variable executor
     |  // variable admin
     |  // variable allKeycodes
     |  // variable getModuleForKeycode
     |  // variable getKeycodeForModule
     |  // variable moduleDependents
     |  // variable getDependentIndex
     |  // variable modulePermissions
     |  // variable activePolicies
     |  // variable getPolicyIndex
     |  // variable hasRole
     |  // variable isRole
     |  // modifier onlyExecutor
     |  // modifier onlyAdmin
     |  // constructor
     |  // function executeAction
     |  // function grantRole
     |  // function revokeRole
     |  // function _installModule
     |  // function _upgradeModule
     |  // function _activatePolicy
     |  // function _deactivatePolicy
     |  // function _migrateKernel
     |  // function _reconfigurePolicies
     |  // function _setPolicyPermissions
     |  // function _pruneFromDependents
 206 |  contract Kernel {
  :  |
     |      // @audit-issue modifier onlyExecutor should come before constructor
 254 |      modifier onlyExecutor() {
 255 |          if (msg.sender != executor) revert Errors.Kernel_OnlyExecutor(msg.sender);
 256 |          _;
 257 |      }
```
GitHub: [40-44](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L40-L44), [77-82](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L77-L82), [130-136](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L130-L136), [254-257](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L254-L257)


## [N-25] Consider adding a block/deny-list

Doing so will significantly increase centralization, but will help to prevent hackers from using stolen tokens.

*Instances: 1*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Consider adding a block/deny-list
  37 |  contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
```
GitHub: [37](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L37)


## [N-26] Contracts and libraries should use fixed compiler versions

To prevent the actual contracts being deployed from behaving differently depending on the compiler version, it is recommended to use fixed solidity versions for contracts and libraries.

Although we can configure a specific version through config (like hardhat, forge config files), it is recommended to **set the fixed version in the solidity pragma directly** before deploying to the mainnet.

*Instances: 12*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L2)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L2)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L2)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L2)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L2)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L2)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L2)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L2)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L2)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L2)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L2)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue File has contracts, use a fixed version of Solidity for deployment
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L2)


## [N-27] Use the latest Solidity version for deployment

When deploying contracts, you should use the latest released version of Solidity (0.8.23). Apart from exceptional cases, only the latest version receives security fixes. Since deployed contracts should not use floating pragmas, I've flagged all instances where a version prior to 0.8.23 is allowed by the version pragma.

*Instances: 12*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L2)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L2)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L2)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L2)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L2)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L2)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L2)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L2)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L2)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L2)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L2)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Allows a compiler other than the latest version (0.8.23)
   2 |  pragma solidity ^0.8.20;
```
GitHub: [2](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L2)


## [N-28] Style: surround top level declarations with two blank lines

As per the [Solidity Style Guide](https://docs.soliditylang.org/en/v0.8.22/style-guide.html#blank-lines):
        
> Surround top level declarations in Solidity source with two blank lines.

Note:  this rule does not apply to `import` directives following another `import` directive.

*Instances: 12*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {IERC20} from "@openzeppelin-contracts/token/ERC20/IERC20.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L2-L4)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {Kernel, Module, Keycode} from "@src/Kernel.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L2-L4)


```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {RentalId, RentalAssetUpdate} from "@src/libraries/RentalStructs.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L2-L4)


```solidity
File: smart-contracts/src/packages/Reclaimer.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {IERC721} from "@openzeppelin-contracts/token/ERC721/IERC721.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L2-L4)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {ECDSA} from "@openzeppelin-contracts/utils/cryptography/ECDSA.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L2-L4)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {Kernel, Policy, Permissions, Keycode} from "@src/Kernel.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L2-L4)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {ZoneParameters} from "@seaport-core/lib/rental/ConsiderationStructs.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L2-L4)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {SafeL2} from "@safe-contracts/SafeL2.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L2-L4)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {BaseGuard} from "@safe-contracts/base/GuardManager.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L2-L4)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {Enum} from "@safe-contracts/common/Enum.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L2-L4)


```solidity
File: smart-contracts/src/Create2Deployer.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {Errors} from "@src/libraries/Errors.sol";
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L2-L4)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Separate top-level declarations with at least two lines
   2 |  pragma solidity ^0.8.20;
   3 |  
   4 |  import {
```
GitHub: [2-4](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L2-L4)


## [N-29] Large multiples of ten should use scientific notation (e.g. `1e6`) rather than decimal literals (e.g. `1000000`), for readability

Use a scientific notation rather than decimal literals (e.g. `1e6` instead of `1000000`), for better code readability.

*Instances: 2*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Use scientific notation (e.g. 1e18) instead
  90 |  return (amount * fee) / 10000;

     |  // @audit-issue Use scientific notation (e.g. 1e18) instead
 382 |  if (feeNumerator > 10000) {
```
GitHub: [90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L90), [382](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L382)


## [N-30] Consider using `delete` rather than assigning zero/false to clear values

The delete keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic.

*Instances: 1*

```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Consider using delete
 342 |  hasRole[addr_][role_] = false;
```
GitHub: [342](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L342)


## [N-31] Unnecessary cast

Unnecessary casts can be removed.

*Instances: 1*

```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue Cast to contract Module is redundant
 514 |  Module module = Module(getModuleForKeycode[allKeycodes[i]]);
```
GitHub: [514](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L514)


## [N-32] Syntax: unnecessary `override`

Starting with Solidity version [0.8.8](https://docs.soliditylang.org/en/v0.8.20/contracts.html#function-overriding), using the `override` keyword when the function solely overrides an interface function, and the function doesn't exist in multiple base contracts, is unnecessary.

*Instances: 17*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue Remove `override`
  68 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {

     |  // @audit-issue Remove `override`
  75 |  function KEYCODE() public pure override returns (Keycode) {
```
GitHub: [68](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L68), [75](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L75)


```solidity
File: smart-contracts/src/modules/Storage.sol

     |  // @audit-issue Remove `override`
  96 |  function VERSION() external pure override returns (uint8 major, uint8 minor) {

     |  // @audit-issue Remove `override`
 103 |  function KEYCODE() public pure override returns (Keycode) {
```
GitHub: [96](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L96), [103](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L103)


```solidity
File: smart-contracts/src/policies/Admin.sol

     |  // @audit-issue Remove `override`
  40 |  function configureDependencies()
  41 |      external
  42 |      override
  43 |      onlyKernel
  44 |      returns (Keycode[] memory dependencies)
  45 |  {

     |  // @audit-issue Remove `override`
  64 |  function requestPermissions()
  65 |      external
  66 |      view
  67 |      override
  68 |      onlyKernel
  69 |      returns (Permissions[] memory requests)
  70 |  {
```
GitHub: [40-45](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L40-L45), [64-70](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Admin.sol#L64-L70)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue Remove `override`
  72 |  function configureDependencies()
  73 |      external
  74 |      override
  75 |      onlyKernel
  76 |      returns (Keycode[] memory dependencies)
  77 |  {

     |  // @audit-issue Remove `override`
  96 |  function requestPermissions()
  97 |      external
  98 |      view
  99 |      override
 100 |      onlyKernel
 101 |      returns (Permissions[] memory requests)
 102 |  {

     |  // @audit-issue Remove `override`
 733 |  function validateOrder(
 734 |      ZoneParameters calldata zoneParams
 735 |  ) external override onlyRole("SEAPORT") returns (bytes4 validOrderMagicValue) {
```
GitHub: [72-77](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L72-L77), [96-102](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L96-L102), [733-735](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L733-L735)


```solidity
File: smart-contracts/src/policies/Factory.sol

     |  // @audit-issue Remove `override`
  73 |  function configureDependencies()
  74 |      external
  75 |      override
  76 |      onlyKernel
  77 |      returns (Keycode[] memory dependencies)
  78 |  {

     |  // @audit-issue Remove `override`
  94 |  function requestPermissions()
  95 |      external
  96 |      view
  97 |      override
  98 |      onlyKernel
  99 |      returns (Permissions[] memory requests)
 100 |  {
```
GitHub: [73-78](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L73-L78), [94-100](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L94-L100)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue Remove `override`
  63 |  function configureDependencies()
  64 |      external
  65 |      override
  66 |      onlyKernel
  67 |      returns (Keycode[] memory dependencies)
  68 |  {

     |  // @audit-issue Remove `override`
  84 |  function requestPermissions()
  85 |      external
  86 |      view
  87 |      override
  88 |      onlyKernel
  89 |      returns (Permissions[] memory requests)
  90 |  {

     |  // @audit-issue Remove `override`
 309 |  function checkTransaction(
 310 |      address to,
 311 |      uint256 value,
 312 |      bytes memory data,
 313 |      Enum.Operation operation,
 314 |      uint256,
 315 |      uint256,
 316 |      uint256,
 317 |      address,
 318 |      address payable,
 319 |      bytes memory,
 320 |      address
 321 |  ) external override {

     |  // @audit-issue Remove `override`
 353 |  function checkAfterExecution(bytes32 txHash, bool success) external override {}
```
GitHub: [63-68](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L63-L68), [84-90](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L84-L90), [309-321](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L309-L321), [353](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L353)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue Remove `override`
  63 |  function configureDependencies()
  64 |      external
  65 |      override
  66 |      onlyKernel
  67 |      returns (Keycode[] memory dependencies)
  68 |  {

     |  // @audit-issue Remove `override`
  87 |  function requestPermissions()
  88 |      external
  89 |      view
  90 |      override
  91 |      onlyKernel
  92 |      returns (Permissions[] memory requests)
  93 |  {
```
GitHub: [63-68](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L63-L68), [87-93](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L87-L93)


## [N-33] Unused `function` definition

Note that there may be cases where a function superficially appears to be used, but this is only because there are multiple definitions of the function in different files. In such cases, the function definition should be moved into a separate file. The instances below are the unused definitions *within the defined scope of analysis*. It is possible these functions are used outside the scope of the analysis.

*Instances: 8*

```solidity
File: smart-contracts/src/packages/Accumulator.sol

     |  // @audit-issue _insert is unused by files in analysis scope
  32 |  function _insert(
  33 |      bytes memory rentalAssets,
  34 |      RentalId rentalId,
  35 |      uint256 rentalAssetAmount
  36 |  ) internal pure {

     |  // @audit-issue _convertToStatic is unused by files in analysis scope
  96 |  function _convertToStatic(
  97 |      bytes memory rentalAssetUpdates
  98 |  ) internal pure returns (RentalAssetUpdate[] memory updates) {
```
GitHub: [32-36](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L32-L36), [96-98](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Accumulator.sol#L96-L98)


```solidity
File: smart-contracts/src/packages/Signer.sol

     |  // @audit-issue _validateFulfiller is unused by files in analysis scope
  76 |  function _validateFulfiller(
  77 |      address intendedFulfiller,
  78 |      address actualFulfiller
  79 |  ) internal pure {

     |  // @audit-issue _validateProtocolSignatureExpiration is unused by files in analysis scope
  94 |  function _validateProtocolSignatureExpiration(uint256 expiration) internal view {

     |  // @audit-issue _recoverSignerFromPayload is unused by files in analysis scope
 107 |  function _recoverSignerFromPayload(
 108 |      bytes32 payloadHash,
 109 |      bytes memory signature
 110 |  ) internal view returns (address) {

     |  // @audit-issue _deriveRentalOrderHash is unused by files in analysis scope
 162 |  function _deriveRentalOrderHash(
 163 |      RentalOrder memory order
 164 |  ) internal view returns (bytes32) {

     |  // @audit-issue _deriveRentPayloadHash is unused by files in analysis scope
 248 |  function _deriveRentPayloadHash(
 249 |      RentPayload memory payload
 250 |  ) internal view returns (bytes32) {
```
GitHub: [76-79](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L76-L79), [94](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L94), [107-110](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L107-L110), [162-164](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L162-L164), [248-250](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L248-L250)


```solidity
File: smart-contracts/src/Kernel.sol

     |  // @audit-issue getModuleAddress is unused by files in analysis scope
 180 |  function getModuleAddress(Keycode keycode_) internal view returns (address) {
```
GitHub: [180](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L180)


## [N-34] Unused import

Some imports are not used, consider removing them.

*Instances: 8*

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

     |  // @audit-issue ItemType is unused
  11 |  ItemType,
```
GitHub: [11](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L11)


```solidity
File: smart-contracts/src/policies/Create.sol

     |  // @audit-issue OrderFulfillment is unused
  25 |  OrderFulfillment,

     |  // @audit-issue RentalId is unused
  31 |  RentalId,

     |  // @audit-issue RentalAssetUpdate is unused
  32 |  RentalAssetUpdate
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L25), [31](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L31), [32](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L32)


```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue e1155_safe_batch_transfer_from_token_id_offset is unused
  25 |  e1155_safe_batch_transfer_from_token_id_offset,
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L25)


```solidity
File: smart-contracts/src/policies/Stop.sol

     |  // @audit-issue ItemType is unused
  25 |  ItemType,

     |  // @audit-issue RentalId is unused
  26 |  RentalId,

     |  // @audit-issue RentalAssetUpdate is unused
  27 |  RentalAssetUpdate
```
GitHub: [25](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L25), [26](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L26), [27](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L27)


## [N-35] Function with unused parameter

An unused function parameter could represent a defect (e.g. a different parameter was used when the intention was to use the unused parameter), or just stale code due to a refactor. For the avoidance of doubt, remove the unused parameter.

*Instances: 1*

```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue `txHash` is unused
     |  // `success` is unused
 353 |  function checkAfterExecution(bytes32 txHash, bool success) external override {}
```
GitHub: [353](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L353)


## [N-36] Named but unused function parameters in an overridden function

`override` function arguments that are unused should have the variable name removed or commented out to avoid compiler warnings.

*Instances: 1*

```solidity
File: smart-contracts/src/policies/Guard.sol

     |  // @audit-issue `txHash` is unused
     |  // `success` is unused
 353 |  function checkAfterExecution(bytes32 txHash, bool success) external override {}
```
GitHub: [353](https://github.com/re-nft/smart-contracts/tree/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L353)
