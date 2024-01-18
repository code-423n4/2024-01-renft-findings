## Gas Findings

Initial gas report was saved with `forge snapshot`.

Total gas saved per issue represent `Overall gas change` from `forge snapshot --diff`.

More details from `forge snapshot --diff` can be found in Gas Findings Details.

|    | Issue | Instances | Total Gas Saved |
|----|-------|:---------:|:---------:|
| [G-01] | Cache Function Calls for Efficiency | 2 | 9887 |
| [G-02] | `abi.encodePacked `is more gas efficient than `abi.encode` | 9 | 6316 |
| [G-03] | Stack variable is only used once | 10 | 1880 |
| [G-04] | Unutilized Named Return Variables Waste Gas Without Optimizer | 2 | 20 |
| [G-05] | Optimize Gas by Using Only Named Returns | 23 | 20 |
| [G-06] | Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes | 8 | 136000 |
| [G-07] | Use `assembly` to write mutable storage values | 42 | 462 |
| [G-08] | Use `revert()` to gain maximum gas savings | 73 | 3650 |
| [G-09] | Use Cached Contracts for Multiple External Calls | 3 | 784 |
| [G-10] | Declare `immutable` as `private` to save gas | 5 | 15000 |
| [G-11] | Avoid updating storage when the value hasn't changed | 7 | 9600 |
| [G-12] | Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct` | 9 | - |


## Gas Findings Details


### [G-01] Cache Function Calls for Efficiency

```bash
test_stopRent_payOrder_proRata_stoppedByLender() (gas: 11 (0.000%)) 
test_stopRent_payOrder_inFull_stoppedByRenter() (gas: -68 (-0.002%)) 
test_StopRent_PayOrder_InFull_StoppedByLender() (gas: -69 (-0.002%)) 
test_stopRentBatch_payOrders_allDifferentLenders() (gas: -207 (-0.002%)) 
test_Success_RewardShare() (gas: -69 (-0.003%)) 
test_Success_RewardShare() (gas: -69 (-0.003%)) 
test_Success_StopRent_NewStopPolicy() (gas: -69 (-0.004%)) 
test_StopRent_BaseOrder() (gas: -69 (-0.004%)) 
test_stopRentBatch_baseOrders_allSameLender() (gas: -207 (-0.005%)) 
test_Success_SettlePayment_PayOrder_NotEnded() (gas: 10 (0.010%)) 
test_Success_SettlePayment_BaseOrder_WithFeeTooSmall() (gas: -69 (-0.061%)) 
test_Success_SettlePayment_BaseOrder_WithFee() (gas: -86 (-0.068%)) 
test_Success_SettlePayment_BaseOrder() (gas: -69 (-0.076%)) 
test_Success_SettlePayment_PayOrder_Ended() (gas: -69 (-0.076%)) 
test_Success_SettlePaymentBatch() (gas: -207 (-0.167%)) 
test_Success_Upgrade() (gas: -2807 (-0.189%)) 
test_Success_Freeze() (gas: -2807 (-0.191%)) 
test_Success_UpgradePaymentEscrow() (gas: -2967 (-0.207%)) 
Overall gas change: -9887 (-0.012%)
```

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

/// @audit cache `orderType.isPayOrder()`
255: if (orderType.isPayOrder() && !isRentalOver) {
268: (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
```
[255](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L255) | [268](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L268)
</details>

### [G-02] `abi.encodePacked `is more gas efficient than `abi.encode`

Tested only one instance due amount of them.

`abi.encode()` pads all elementary types to 32 bytes, whereas `abi.encodePacked()` will only use the minimal required memory to encode the data.
```solidity
    function testPacking() public pure returns (bytes memory) {
        // return abi.encode("string"); // 1120 gas
        // return abi.encodePacked("string"); // 913 gas
    }
```

```bash
test_Reverts_RenterIsNotWhitelisted() (gas: -73 (-0.004%)) 
test_stopRentBatch_payOrders_allDifferentLenders() (gas: -668 (-0.008%)) 
test_Reverts_TrainSelector_Callable_NonRentedOnly() (gas: -189 (-0.009%)) 
test_Success_RenterIsWhitelisted() (gas: -189 (-0.009%)) 
test_Reverts_TrainSelector_Callable_RetireSelector_NotCallable() (gas: -189 (-0.009%)) 
test_Reverts_ChangeTeam_NotCallable() (gas: -189 (-0.010%)) 
test_Reverts_TrainSelector_NotCallable() (gas: -189 (-0.010%)) 
test_stopRent_payOrder_proRata_stoppedByLender() (gas: -299 (-0.010%)) 
test_stopRent_payOrder_inFull_stoppedByRenter() (gas: -299 (-0.010%)) 
test_StopRent_PayOrder_InFull_StoppedByLender() (gas: -299 (-0.010%)) 
test_Success_RewardShare() (gas: -233 (-0.011%)) 
test_Success_RewardShare() (gas: -232 (-0.011%)) 
test_Success_Rent_PayOrder() (gas: -325 (-0.011%)) 
test_Reverts_StopRent_OldStopPolicy() (gas: -189 (-0.011%)) 
test_Success_StopRent_NewStopPolicy() (gas: -231 (-0.013%)) 
test_StopRent_BaseOrder() (gas: -231 (-0.014%)) 
test_stopRentBatch_baseOrders_allSameLender() (gas: -668 (-0.015%)) 
test_Success_Rent_BaseOrder_ERC721() (gas: -257 (-0.016%)) 
test_Success_Rent_BaseOrder_ERC1155() (gas: -257 (-0.016%)) 
test_Success_Rent_BaseOrders() (gas: -770 (-0.017%)) 
test_Fuzz_SettlePaymentInFull(uint256,bool) (gas: -20 (-0.024%)) 
test_Success_ProcessBaseOrderConsideration() (gas: -32 (-0.162%)) 
test_Success_ProcessBaseOrderOffer() (gas: -32 (-0.183%)) 
test_Success_ConvertToItems_BaseOrder() (gas: -64 (-0.227%)) 
test_Success_ProcessPayOrderOffer() (gas: -64 (-0.233%)) 
test_Success_ConvertToItems_PayeeOrder() (gas: -64 (-0.236%)) 
test_Success_ConvertToItems_PayOrder() (gas: -64 (-0.242%)) 
Overall gas change: -6316 (-0.007%)
```

<details>
<summary><i>9 issue instances in 2 files:</i></summary>

```solidity
File: smart-contracts/src/packages/Signer.sol

129: abi.encode(
                    _ITEM_TYPEHASH,
                    item.itemType,
                    item.settleTo,
                    item.token,
                    item.amount,
                    item.identifier
                )
151: abi.encode(_HOOK_TYPEHASH, hook.target, hook.itemIndex, hook.extraData)
183: abi.encode(
                    _RENTAL_ORDER_TYPEHASH,
                    order.seaportOrderHash,
                    keccak256(abi.encodePacked(itemHashes)),
                    keccak256(abi.encodePacked(hookHashes)),
                    order.orderType,
                    order.lender,
                    order.renter,
                    order.startTimestamp,
                    order.endTimestamp
                )
208: abi.encode(_ORDER_FULFILLMENT_TYPEHASH, fulfillment.recipient)
233: abi.encode(
                    _ORDER_METADATA_TYPEHASH,
                    metadata.rentDuration,
                    keccak256(abi.encodePacked(hookHashes))
                )
254: abi.encode(
                    _RENT_PAYLOAD_TYPEHASH,
                    _deriveOrderFulfillmentHash(payload.fulfillment),
                    _deriveOrderMetadataHash(payload.metadata),
                    payload.expiration,
                    payload.intendedFulfiller
                )
280: abi.encode(
                    _eip712DomainTypeHash,
                    _nameHash,
                    _versionHash,
                    block.chainid,
                    address(this)
                )
374: abi.encode(rentalOrderTypeString, hookTypeString, itemTypeString)
```
[129](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L129) | [151](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L151) | [183](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L183) | [208](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L208) | [233](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L233) | [254](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L254) | [280](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L280) | [374](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L374)

```solidity
File: smart-contracts/src/policies/Factory.sol

184: abi.encode(STORE.totalSafes() + 1, block.chainid)
```
[184](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L184)
</details>

### [G-03] Stack variable is only used once

Tested only one instance due amount of them.

```bash
test_stopRent_payOrder_proRata_stoppedByLender() (gas: -5 (-0.000%)) 
test_Success_SettlePayment_PayOrder_NotEnded() (gas: -6 (-0.006%)) 
test_Fuzz_SettlePaymentProRata(uint256,uint256,uint256) (gas: -7 (-0.006%)) 
test_Fuzz_SettlePaymentInFull(uint256,bool) (gas: -20 (-0.024%)) 
test_Success_Upgrade() (gas: -600 (-0.040%)) 
test_Success_Freeze() (gas: -600 (-0.041%)) 
test_Success_UpgradePaymentEscrow() (gas: -600 (-0.042%)) 
test_Success_CalculatePaymentProRata() (gas: -42 (-0.217%)) 
Overall gas change: -1880 (-0.002%)
```

<details>
<summary><i>10 issue instances in 5 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

/// @audit - `numerator` variable
138: uint256 numerator = (amount * elapsedTime) * 1000
/// @audit - `settleToAddress` variable
198: address settleToAddress = settleTo == SettleTo.LENDER ? lender : renter
/// @audit - `paymentFee` variable
244: uint256 paymentFee = _calculateFee(paymentAmount)
/// @audit - `syncedBalance` variable
399: uint256 syncedBalance = balanceOf[token]
/// @audit - `trueBalance` variable
402: uint256 trueBalance = IERC20(token).balanceOf(address(this))
```
[138](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L138) | [198](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L198) | [244](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L244) | [399](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L399) | [402](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L402)

```solidity
File: smart-contracts/src/modules/Storage.sol

/// @audit - `rentalId` variable
124: RentalId rentalId = RentalUtils.getItemPointer(recipient, token, identifier)
```
[124](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L124)

```solidity
File: smart-contracts/src/packages/Signer.sol

/// @audit - `digest` variable
112: bytes32 digest = _DOMAIN_SEPARATOR.toTypedDataHash(payloadHash)
```
[112](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L112)

```solidity
File: smart-contracts/src/policies/Create.sol

/// @audit - `stringErrorCode` variable
509: string memory stringErrorCode = LibString.toString(errorCode)
```
[509](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L509)

```solidity
File: smart-contracts/src/policies/Guard.sol

/// @audit - `stringErrorCode` variable
174: string memory stringErrorCode = LibString.toString(errorCode)
/// @audit - `isActive` variable
335: bool isActive = STORE.hookOnTransaction(hook)
```
[174](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L174) | [335](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L335)
</details>

### [G-04] Unutilized Named Return Variables Waste Gas Without Optimizer

```diff
-   function VERSION() external pure override returns (uint8 major, uint8 minor) {
+   function VERSION() external pure override returns (uint8, uint8) {
```

```bash
test_Fuzz_SettlePaymentInFull(uint256,bool) (gas: -20 (-0.024%)) 
Overall gas change: -20 (-0.000%)
```

<details>
<summary><i>2 issue instances in 2 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

/// @audit - Redundant `return (1, 0);` statement in functions with named return variables
68: function VERSION() external pure override returns (uint8 major, uint8 minor) {
```
[68](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L68)

```solidity
File: smart-contracts/src/modules/Storage.sol

/// @audit - Redundant `return (1, 0);` statement in functions with named return variables
96: function VERSION() external pure override returns (uint8 major, uint8 minor) {
```
[96](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L96)
</details>



### [G-05] Optimize Gas by Using Only Named Returns

Tested only one instance due amount of them.

Example:
```solidity
/// 985 gas cost
function add(uint256 x, uint256 y) public pure returns (uint256) {
    return x + y;
}
/// 941 gas cost
function addNamed(uint256 x, uint256 y) public pure returns (uint256 res) {
    res = x + y;
}
```

```diff
-    function _calculateFee(uint256 amount) internal view returns (uint256) {
-        // Uses 10,000 as a denominator for the fee.
-        return (amount * fee) / 10000;
-    }

+    function _calculateFee(uint256 amount) internal view returns (uint256 result) {
+        // Uses 10,000 as a denominator for the fee.
+        result = (amount * fee) / 10000;
+    }
```

```bash
test_Fuzz_SettlePaymentInFull(uint256,bool) (gas: -20 (-0.024%)) 
Overall gas change: -20 (-0.000%)
```

<details>
<summary><i>23 issue instances in 6 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

75: function KEYCODE() public pure override returns (Keycode) {
88: function _calculateFee(uint256 amount) internal view returns (uint256) {
```
[75](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L75) | [88](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L88)

```solidity
File: smart-contracts/src/modules/Storage.sol

103: function KEYCODE() public pure override returns (Keycode) {
118: function isRentedOut(
        address recipient,
        address token,
        uint256 identifier
    ) external view returns (bool) {
135: function contractToHook(address to) external view returns (address) {
149: function hookOnTransaction(address hook) external view returns (bool) {
159: function hookOnStart(address hook) external view returns (bool) {
169: function hookOnStop(address hook) external view returns (bool) {
```
[103](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L103) | [118](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L118) | [135](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L135) | [149](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L149) | [159](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L159) | [169](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L169)

```solidity
File: smart-contracts/src/packages/Signer.sol

107: function _recoverSignerFromPayload(
        bytes32 payloadHash,
        bytes memory signature
    ) internal view returns (address) {
125: function _deriveItemHash(Item memory item) internal view returns (bytes32) {
147: function _deriveHookHash(Hook memory hook) internal view returns (bytes32) {
162: function _deriveRentalOrderHash(
        RentalOrder memory order
    ) internal view returns (bytes32) {
204: function _deriveOrderFulfillmentHash(
        OrderFulfillment memory fulfillment
    ) internal view returns (bytes32) {
218: function _deriveOrderMetadataHash(
        OrderMetadata memory metadata
    ) internal view returns (bytes32) {
248: function _deriveRentPayloadHash(
        RentPayload memory payload
    ) internal view returns (bytes32) {
273: function _deriveDomainSeparator(
        bytes32 _eip712DomainTypeHash,
        bytes32 _nameHash,
        bytes32 _versionHash
    ) internal view virtual returns (bytes32) {
```
[107](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L107) | [125](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L125) | [147](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L147) | [162](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L162) | [204](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L204) | [218](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L218) | [248](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L248) | [273](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L273)

```solidity
File: smart-contracts/src/policies/Create.sol

117: function domainSeparator() external view returns (bytes32) {
126: function getRentalOrderHash(
        RentalOrder memory order
    ) external view returns (bytes32) {
137: function getRentPayloadHash(
        RentPayload memory payload
    ) external view returns (bytes32) {
148: function getOrderMetadataHash(
        OrderMetadata memory metadata
    ) external view returns (bytes32) {
```
[117](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L117) | [126](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L126) | [137](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L137) | [148](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L148)

```solidity
File: smart-contracts/src/Create2Deployer.sol

84: function getCreate2Address(
        bytes32 salt,
        bytes memory initCode
    ) public view returns (address) {
```
[84](https://github.com/re-nft/smart-contracts/blob/main/src/Create2Deployer.sol#L84)

```solidity
File: smart-contracts/src/Kernel.sol

90: function KEYCODE() public pure virtual returns (Keycode);
180: function getModuleAddress(Keycode keycode_) internal view returns (address) {
```
[90](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L90) | [180](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L180)
</details>

### [G-06] Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes

Boolean variables in Solidity are more expensive than `uint256` or any type that takes up a full word, due to additional gas costs associated with write operations.
When using boolean variables, each write operation emits an extra SLOAD to read the slot's contents, replace the bits taken up by the boolean, and then write back.
This process cannot be disabled and leads to extra gas consumption.

By using `uint256(1)` and `uint256(2)` for representing true and false states, you can avoid a `Gwarmaccess` (100 gas) cost and also avoid a `Gsset` (20000 gas) cost when changing from `false` to `true`, after having been `true` in the past.
This approach helps in optimizing gas usage, making your contract more cost-effective.

[Usage in OpenZeppelin ReentrancyGuard.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27)

<details>
<summary><i>8 issue instances in 3 files:</i></summary>

```solidity
File: smart-contracts/src/modules/Storage.sol

20: mapping(bytes32 orderHash => bool isActive) public orders;
55: mapping(address delegate => bool isWhitelisted) public whitelistedDelegates;
58: mapping(address extension => bool isWhitelisted) public whitelistedExtensions;
```
[20](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L20) | [55](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L55) | [58](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L58)

```solidity
File: smart-contracts/src/Create2Deployer.sol

16: mapping(address => bool) public deployed;
```
[16](https://github.com/re-nft/smart-contracts/blob/main/src/Create2Deployer.sol#L16)

```solidity
File: smart-contracts/src/Kernel.sol

116: bool public isActive;
221: mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
        public modulePermissions; // for policy addr, check if they have permission to call the function in the module.
229: mapping(address => mapping(Role => bool)) public hasRole;
230: mapping(Role => bool) public isRole;
```
[116](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L116) | [221](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L221) | [229](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L229) | [230](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L230)
</details>

### [G-07] Use `assembly` to write mutable storage values

Writing to storage using `assembly` is more gas efficient.
```solidity
    function writeStorage() external {
        // storageNumber = 10; // 2358 gas
        // assembly {
        //     sstore(storageNumber.slot, 10) // 2350 gas
        // }
        // storageAddr = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc3; // 2411 gas
        // assembly {
        //     sstore(storageAddr.slot, 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc3) // 2350 gas
        // }
    }
```

<details>
<summary><i>42 issue instances in 9 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

61: kernel = kernel_
62: initialized = true
387: fee = feeNumerator
```
[61](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L61) | [62](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L62) | [387](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L387)

```solidity
File: smart-contracts/src/modules/Storage.sol

89: kernel = kernel_
90: initialized = true
194: orders[orderHash] = true
279: deployedSafes[safe] = newSafeCount
282: totalSafes = newSafeCount
302: _contractToHook[to] = hook
325: hookStatus[hook] = bitmap
338: whitelistedDelegates[delegate] = isEnabled
351: whitelistedExtensions[extension] = isEnabled
```
[89](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L89) | [90](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L90) | [194](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L194) | [279](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L279) | [282](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L282) | [302](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L302) | [325](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L325) | [338](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L338) | [351](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L351)

```solidity
File: smart-contracts/src/policies/Admin.sol

49: STORE = Storage(getModuleAddress(toKeycode("STORE")))
52: ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")))
```
[49](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Admin.sol#L49) | [52](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Admin.sol#L52)

```solidity
File: smart-contracts/src/policies/Create.sol

81: STORE = Storage(getModuleAddress(toKeycode("STORE")))
84: ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")))
```
[81](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L81) | [84](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L84)

```solidity
File: smart-contracts/src/policies/Factory.sol

82: STORE = Storage(getModuleAddress(toKeycode("STORE")))
```
[82](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L82)

```solidity
File: smart-contracts/src/policies/Guard.sol

72: STORE = Storage(getModuleAddress(toKeycode("STORE")))
```
[72](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L72)

```solidity
File: smart-contracts/src/policies/Stop.sol

72: STORE = Storage(getModuleAddress(toKeycode("STORE")))
75: ESCRW = PaymentEscrow(getModuleAddress(toKeycode("ESCRW")))
```
[72](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L72) | [75](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L75)

```solidity
File: smart-contracts/src/Create2Deployer.sol

50: deployed[targetDeploymentAddress] = true
```
[50](https://github.com/re-nft/smart-contracts/blob/main/src/Create2Deployer.sol#L50)

```solidity
File: smart-contracts/src/Kernel.sol

34: kernel = kernel_
55: kernel = newKernel_
193: isActive = activate_
243: executor = _executor
244: admin = _admin
296: executor = target_
298: admin = target_
319: isRole[role_] = true
322: hasRole[addr_][role_] = true
342: hasRole[addr_][role_] = false
366: getModuleForKeycode[keycode] = newModule_
369: getKeycodeForModule[newModule_] = keycode
397: getKeycodeForModule[oldModule] = Keycode.wrap(bytes5(0))
400: getKeycodeForModule[newModule_] = keycode
403: getModuleForKeycode[keycode] = newModule_
431: getPolicyIndex[policy_] = activePolicies.length - 1
445: getDependentIndex[keycode][policy_] = moduleDependents[keycode].length - 1
472: activePolicies[idx] = lastPolicy
479: getPolicyIndex[lastPolicy] = idx
569: modulePermissions[request.keycode][policy_][request.funcSelector] = grant_
611: getDependentIndex[keycode][lastPolicy] = origIndex
```
[34](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L34) | [55](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L55) | [193](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L193) | [243](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L243) | [244](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L244) | [296](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L296) | [298](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L298) | [319](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L319) | [322](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L322) | [342](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L342) | [366](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L366) | [369](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L369) | [397](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L397) | [400](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L400) | [403](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L403) | [431](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L431) | [445](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L445) | [472](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L472) | [479](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L479) | [569](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L569) | [611](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L611)
</details>

### [G-08] Use `revert()` to gain maximum gas savings

If you dont need Error messages, or you want gain maximum gas savings - `revert()` is a cheapest way to revert transaction in terms of gas.
```solidity
    revert(); // 117 gas 
    require(false); // 132 gas
    revert CustomError(); // 157 gas
    assert(false); // 164 gas
    revert("Custom Error"); // 406 gas
    require(false, "Custom Error"); // 421 gas
```


<details>
<summary><i>73 issue instances in 10 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

116: revert Errors.PaymentEscrowModule_PaymentTransferFailed(token, to, value)
279: revert Errors.Shared_OrderTypeNotSupported(uint8(orderType))
367: revert Errors.PaymentEscrow_ZeroPayment()
383: revert Errors.PaymentEscrow_InvalidFeeNumerator()
```
[116](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L116) | [279](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L279) | [367](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L367) | [383](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L383)

```solidity
File: smart-contracts/src/modules/Storage.sol

222: revert Errors.StorageModule_OrderDoesNotExist(orderHash)
252: revert Errors.StorageModule_OrderDoesNotExist(orderHashes[i])
296: revert Errors.StorageModule_NotContract(to)
299: revert Errors.StorageModule_NotContract(hook)
318: revert Errors.StorageModule_NotContract(hook)
322: revert Errors.StorageModule_InvalidHookStatusBitmap(bitmap)
```
[222](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L222) | [252](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L252) | [296](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L296) | [299](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L299) | [318](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L318) | [322](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L322)

```solidity
File: smart-contracts/src/packages/Reclaimer.sol

74: revert Errors.ReclaimerPackage_OnlyDelegateCallAllowed()
81: revert Errors.ReclaimerPackage_OnlyRentalSafeAllowed(
                rentalOrder.rentalWallet
            )
```
[74](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Reclaimer.sol#L74) | [81](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Reclaimer.sol#L81)

```solidity
File: smart-contracts/src/packages/Signer.sol

82: revert Errors.SignerPackage_UnauthorizedFulfiller(
                actualFulfiller,
                intendedFulfiller
            )
97: revert Errors.SignerPackage_SignatureExpired(block.timestamp, expiration)
```
[82](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L82) | [97](https://github.com/re-nft/smart-contracts/blob/main/src/packages/Signer.sol#L97)

```solidity
File: smart-contracts/src/policies/Create.sol

202: revert Errors.CreatePolicy_OfferCountZero()
223: revert Errors.CreatePolicy_SeaportItemTypeNotSupported(offer.itemType)
297: revert Errors.CreatePolicy_SeaportItemTypeNotSupported(offer.itemType)
312: revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments)
333: revert Errors.CreatePolicy_ConsiderationCountZero()
343: revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                    consideration.itemType
                )
389: revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                    consideration.itemType
                )
397: revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments)
434: revert Errors.CreatePolicy_ConsiderationCountNonZero(
                    considerations.length
                )
443: revert Errors.CreatePolicy_OfferCountNonZero(offers.length)
451: revert Errors.Shared_OrderTypeNotSupported(uint8(orderType))
481: revert Errors.Shared_DisabledHook(target)
492: revert Errors.Shared_NonRentalHookItem(itemIndex)
506: revert Errors.Shared_HookFailString(revertReason)
512: revert Errors.Shared_HookFailString(
                    string.concat("Hook reverted: Panic code ", stringErrorCode)
                )
517: revert Errors.Shared_HookFailBytes(revertData)
632: revert Errors.CreatePolicy_RentDurationZero()
637: revert Errors.CreatePolicy_InvalidOrderMetadataHash()
650: revert Errors.CreatePolicy_InvalidRentalSafe(safe)
655: revert Errors.CreatePolicy_InvalidSafeOwner(owner, safe)
671: revert Errors.CreatePolicy_UnexpectedTokenRecipient(
                execution.itemType,
                execution.token,
                execution.identifier,
                execution.amount,
                execution.recipient,
                expectedRecipient
            )
709: revert Errors.CreatePolicy_SeaportItemTypeNotSupported(
                    execution.itemType
                )
767: revert Errors.CreatePolicy_UnauthorizedCreatePolicySigner()
```
[202](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L202) | [223](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L223) | [297](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L297) | [312](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L312) | [333](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L333) | [343](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L343) | [389](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L389) | [397](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L397) | [434](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L434) | [443](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L443) | [451](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L451) | [481](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L481) | [492](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L492) | [506](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L506) | [512](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L512) | [517](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L517) | [632](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L632) | [637](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L637) | [650](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L650) | [655](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L655) | [671](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L671) | [709](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L709) | [767](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L767)

```solidity
File: smart-contracts/src/policies/Factory.sol

144: revert Errors.FactoryPolicy_InvalidSafeThreshold(threshold, owners.length)
```
[144](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L144)

```solidity
File: smart-contracts/src/policies/Guard.sol

134: revert Errors.GuardPolicy_UnauthorizedSelector(selector)
146: revert Errors.GuardPolicy_UnauthorizedExtension(extension)
171: revert Errors.Shared_HookFailString(revertReason)
177: revert Errors.Shared_HookFailString(
                string.concat("Hook reverted: Panic code ", stringErrorCode)
            )
182: revert Errors.Shared_HookFailBytes(revertData)
271: revert Errors.GuardPolicy_UnauthorizedSelector(
                    shared_set_approval_for_all_selector
                )
281: revert Errors.GuardPolicy_UnauthorizedSelector(
                    e1155_safe_batch_transfer_from_selector
                )
288: revert Errors.GuardPolicy_UnauthorizedSelector(
                    gnosis_safe_set_guard_selector
                )
325: revert Errors.GuardPolicy_UnauthorizedDelegateCall(to)
330: revert Errors.GuardPolicy_FunctionSelectorRequired()
```
[134](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L134) | [146](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L146) | [171](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L171) | [177](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L177) | [182](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L182) | [271](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L271) | [281](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L281) | [288](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L288) | [325](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L325) | [330](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L330)

```solidity
File: smart-contracts/src/policies/Stop.sol

141: revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender)
149: revert Errors.StopPolicy_CannotStopOrder(block.timestamp, msg.sender)
154: revert Errors.Shared_OrderTypeNotSupported(uint8(orderType))
181: revert Errors.StopPolicy_ReclaimFailed()
211: revert Errors.Shared_DisabledHook(target)
222: revert Errors.Shared_NonRentalHookItem(itemIndex)
236: revert Errors.Shared_HookFailString(revertReason)
242: revert Errors.Shared_HookFailString(
                    string.concat("Hook reverted: Panic code ", stringErrorCode)
                )
247: revert Errors.Shared_HookFailBytes(revertData)
```
[141](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L141) | [149](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L149) | [154](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L154) | [181](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L181) | [211](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L211) | [222](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L222) | [236](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L236) | [242](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L242) | [247](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Stop.sol#L247)

```solidity
File: smart-contracts/src/Create2Deployer.sol

38: revert Errors.Create2Deployer_UnauthorizedSender(msg.sender, salt)
46: revert Errors.Create2Deployer_AlreadyDeployed(targetDeploymentAddress, salt)
68: revert Errors.Create2Deployer_MismatchedDeploymentAddress(
                targetDeploymentAddress,
                deploymentAddress
            )
```
[38](https://github.com/re-nft/smart-contracts/blob/main/src/Create2Deployer.sol#L38) | [46](https://github.com/re-nft/smart-contracts/blob/main/src/Create2Deployer.sol#L46) | [68](https://github.com/re-nft/smart-contracts/blob/main/src/Create2Deployer.sol#L68)

```solidity
File: smart-contracts/src/Kernel.sol

42: revert Errors.KernelAdapter_OnlyKernel(msg.sender)
79: revert Errors.Module_PolicyNotAuthorized(msg.sender)
133: revert Errors.Policy_OnlyRole(role)
183: revert Errors.Policy_ModuleDoesNotExist(keycode_)
255: revert Errors.Kernel_OnlyExecutor(msg.sender)
263: revert Errors.Kernel_OnlyAdmin(msg.sender)
313: revert Errors.Kernel_AddressAlreadyHasRole(addr_, role_)
335: revert Errors.Kernel_RoleDoesNotExist(role_)
339: revert Errors.Kernel_AddressDoesNotHaveRole(addr_, role_)
362: revert Errors.Kernel_ModuleAlreadyInstalled(keycode)
393: revert Errors.Kernel_InvalidModuleUpgrade(keycode)
421: revert Errors.Kernel_PolicyAlreadyApproved(address(policy_))
458: revert Errors.Kernel_PolicyNotApproved(address(policy_))
```
[42](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L42) | [79](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L79) | [133](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L133) | [183](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L183) | [255](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L255) | [263](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L263) | [313](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L313) | [335](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L335) | [339](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L339) | [362](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L362) | [393](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L393) | [421](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L421) | [458](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L458)
</details>

### [G-09] Use Cached Contracts for Multiple External Calls

When function makes multiple calls to the same external contract, it is more gas-efficient to use a local copy of the contract.
This is because the EVM will cache the contract in memory, and subsequent calls will be cheaper.
It's especially true for contracts that are large and/or have many functions.
```solidity
    // local cache -> 6561 gas
    IToken localCache = storageContract;
    localCache.externalCall();
    localCache.externalCall();

    // direct call 6683 gas
    storageContract.externalCall();
    storageContract.externalCall();
```

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: smart-contracts/src/policies/Guard.sol

/// @audit function checkTransaction() make external call of `STORE` - 3 times
324: if (operation == Enum.Operation.DelegateCall && !STORE.whitelistedDelegates(to)) {
334: address hook = STORE.contractToHook(to);
335: bool isActive = STORE.hookOnTransaction(hook);
```
[324](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L324) | [334](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L334) | [335](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L335)
</details>

### [G-10] Declare `immutable` as `private` to save gas

The compiler doesn't need to create non-payable getter functions for deployment calldata, store the bytes of the value outside of where it's used, or add another entry to the method ID table, saving 3406-3606 gas in deployment.

<details>
<summary><i>5 issue instances in 1 files:</i></summary>

```solidity
File: smart-contracts/src/policies/Factory.sol

31: Stop public immutable stopPolicy;
32: Guard public immutable guardPolicy;
35: TokenCallbackHandler public immutable fallbackHandler;
36: SafeProxyFactory public immutable safeProxyFactory;
37: SafeL2 public immutable safeSingleton;
```
[31](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L31) | [32](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L32) | [35](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L35) | [36](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L36) | [37](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Factory.sol#L37)
</details>

### [G-11] Avoid updating storage when the value hasn't changed

A check regarding whether the current value and the new value are the same should be added.
This helps prevent unnecessary state changes and events in case the new value is the same as the current value.

<details>
<summary><i>7 issue instances in 5 files:</i></summary>

```solidity
File: smart-contracts/src/modules/PaymentEscrow.sol

387: fee = feeNumerator;
```
[387](https://github.com/re-nft/smart-contracts/blob/main/src/modules/PaymentEscrow.sol#L387)

```solidity
File: smart-contracts/src/modules/Storage.sol

/// @audit Missing `hook` check before state change
302: _contractToHook[to] = hook;
/// @audit Missing `bitmap` check before state change
325: hookStatus[hook] = bitmap;
```
[302](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L302) | [325](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L325)

```solidity
File: smart-contracts/src/policies/Admin.sol

/// @audit Missing `feeNumerator` check before state change
174: ESCRW.setFee(feeNumerator);
```
[174](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Admin.sol#L174)

```solidity
File: smart-contracts/src/policies/Guard.sol

/// @audit Missing `hook` check before state change
363: STORE.updateHookPath(to, hook);
/// @audit Missing `bitmap` check before state change
377: STORE.updateHookStatus(hook, bitmap);
```
[363](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L363) | [377](https://github.com/re-nft/smart-contracts/blob/main/src/policies/Guard.sol#L377)

```solidity
File: smart-contracts/src/Kernel.sol

/// @audit Missing `newKernel_` check before state change
55: kernel = newKernel_;
```
[55](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L55)
</details>

### [G-12] Multiple `address`/ID mappings can be combined into a single `mapping` of an `address`/ID to a `struct`

Combining multiple address/ID mappings into a single mapping to a struct can lead to gas savings.
By refactoring multiple mappings into a singular mapping with a struct, you can save on storage slots, which in turn can reduce the gas cost in certain operations.
Prioritize this refactor if optimizing gas is a primary concern for your contract's operations.

<details>
<summary><i>9 issue instances in 2 files:</i></summary>

```solidity
File: smart-contracts/src/modules/Storage.sol

33: mapping(address safe => uint256 nonce) public deployedSafes
44: mapping(address to => address hook) internal _contractToHook
47: mapping(address hook => uint8 enabled) public hookStatus
55: mapping(address delegate => bool isWhitelisted) public whitelistedDelegates
58: mapping(address extension => bool isWhitelisted) public whitelistedExtensions
```
[33](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L33) | [44](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L44) | [47](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L47) | [55](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L55) | [58](https://github.com/re-nft/smart-contracts/blob/main/src/modules/Storage.sol#L58)

```solidity
File: smart-contracts/src/Kernel.sol

213: mapping(Keycode => Module) public getModuleForKeycode
217: mapping(Keycode => Policy[]) public moduleDependents
218: mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex
221: mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))
        public modulePermissions
```
[213](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L213) | [217](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L217) | [218](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L218) | [221](https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L221)
</details>