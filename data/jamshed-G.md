## \[G-01\] Amounts should be checked for 0 before calling a transfer

It is generally a good practice to check for zero values before making any transfers in smart contract functions. This can help to avoid unnecessary external calls and can save gas costs.

Checking for zero values is especially important when transferring tokens or ether, as sending these assets to an address with a zero value will result in the loss of those assets.

In Solidity, you can check whether a value is zero by using the == operator. Here's an example of how you can check for a zero value before making a transfer:

```
function transfer(address payable recipient, uint256 amount) public {
    require(amount > 0, "Amount must be greater than zero");
    recipient.transfer(amount);
}
```

In the above example, we check to make sure that the amount parameter is greater than zero before making the transfer to the recipient address. If the amount is zero or negative, the function will revert and the transfer will not be made.

```
_safeTransfer(token, settleToAddress, amount);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L20



## \[G-02\] Avoid using lower than 256bit variables whenever possible

Using `uint8` instead of `uint256` in Solidity can be less efficient and potentially more costly in certain contexts, primarily **due to the way the Ethereum Virtual Machine (EVM) operates.**

**The EVM operates with a word size of 256 bits.** This means that operations on 256-bit integers (`uint256`) are generally the most efficient, as they align with the EVM's native word size. When you use smaller integers like `uint8`, Solidity often needs to perform additional operations to align these smaller types with the EVM's 256-bit word size. **This can result in more complex and less efficient code.**

While using smaller types like `uint8` can be beneficial for optimizing storage (since **multiple** `uint8` **variables can be packed into a single 256-bit storage slot**), this benefit is typically seen only in storage and not in memory or stack operations.

On top of it, for computations, the conversion to and from `uint256` can negate the storage savings.

```
uint8 public a = 12;

uint256 public b = 13;

uint8 public c = 14;

// It can lead to inefficiencies and increased gas costs

// due to the EVM's optimization for 256-bit operations.
```

In summary, while using `uint8` may seem like a good way to save space and potentially reduce costs, but in practice, **it can lead to inefficiencies and increased gas costs due to the EVM's optimization for 256-bit operations.**

```
uint256 public a = 12;

uint256 public b = 14;

uint256 public c = 13;

// Better solution
```

You can create transactions that invoke a function `f(uint8 x)` with a raw byte argument of `0xff000001` and `0x00000001`. Both are supplied to the contract and will appear as the number `1` to `x`. However, `msg.data` will differ in each case. Therefore, if your code implements things like `keccak256(msg.data)` running any logic, you will get different results.

&nbsp;

```
revert Errors.Shared_OrderTypeNotSupported(uint8(orderType));
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L279

```
mapping(address hook => uint8 enabled) public hookStatus;
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L47

## \[G-03\] Use hardcode address instead `address(this)`

Instead of using `address(this)`, it is more gas-efficient to pre-calculate and use the hardcoded `address`. Foundry’s script.sol and solmate’s `LibRlp.sol` contracts can help achieve this.

```
uint256 trueBalance = IERC20(token).balanceOf(address(this));
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L402

```
original = address(this);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L23

```
IERC721(item.token).safeTransferFrom(address(this), recipient, item.identifier);
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L33

```
address(this),
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L44

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L73

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L80

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L124C9-L127C53

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Factory.sol#L163

## \[G-04\] Use assembly to validate `msg.sender`

We can use assembly to efficiently validate msg.sender for the didPay and uniswapV3SwapCallback functions with the least amount of opcodes necessary. Additionally, we can use xor() instead of iszero(eq()), saving 3 gas. We can also potentially save gas on the unhappy path by using scratch space to store the error selector, potentially avoiding memory expansion costs.

```
bool isLender = expectedLender == msg.sender;
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L135

&nbsp;

## \[G-05\] abi.encode() is less efficient than abi.encodePacked()

In terms of efficiency, abi.encodePacked() is generally considered to be more gas-efficient than abi.encode(), because it skips the step of adding function signatures and other metadata to the encoded data. However, this comes at the cost of reduced safety, as abi.encodePacked() does not perform any type checking or padding of data.

&nbsp;

```
abi.encode(
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L129

```
abi.encode(_HOOK_TYPEHASH, hook.target, hook.itemIndex, hook.extraData)
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L151

&nbsp;

## \[G-06\] bytes constants are more eficient than string constans

If the data can fit in 32 bytes, the bytes32 data type can be used instead of bytes or strings, as it is less robust in terms of robustness.

```
string internal constant _NAME = "ReNFT-Rentals";
    string internal constant _VERSION = "1.0.0";
```

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L25C5-L26C49