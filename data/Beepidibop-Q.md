## [NC-1] `Signer`: Some EIP-712 Type Hashes Don't Follow the Standard

EIP-712 specifies that 

> If the struct type references other struct types (and these in turn reference even more struct types), then the set of referenced struct types is collected, sorted by name and appended to the encoding. An example encoding is `Transaction(Person from,Person to,Asset tx)Asset(address token,uint256 amount)Person(address wallet,string name)`. [link](https://eips.ethereum.org/EIPS/eip-712#definition-of-encodetype)

There are a few type hashes don't include all of the referenced struct type strings or don't have them in the right order.

Here is a list of type strings that don't follow the standard:
- `rentPayloadTypeHash` - missing Hook, structs not ordered alphabetically
- `orderMetadataTypeHash` - missing Hook

### Recommendation

Append the struct type strings to the type strings.

### Link To Affected Code

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L394-L400
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L406

## [INFO-1] `Kernel`: Inconsistent Use of `@` for Import Paths

Some import paths for have `@` prefix while some does not for the same paths. It seems like other files all import with the `@` prefix, only the `Kernel.sol` file is missing the `@`.

e.g. 

```
import {Errors} from "@src/libraries/Errors.sol";
import {Events} from "src/libraries/Events.sol";
```

### Recommendation

Add the `@` prefix in `Kernel.sol`

### Link To Affected Code

https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L12

## [INFO-2] `Kernel`: `isRole` Can Be `true` When There Isn't an Address With the Role

`grantRole()` checks if the role has previously been granted and set `isRole` to true on the first granting of a role. But `revokeRole()` doesn't check if the address it's removing the role from is the last address with said role.

### Recommendation

No actions needed, the gas usage isn't worth it. Since the project will need to introduce more storage slots to store how many addresses has a certain role and change its counter on `grantRole()` and `revokeRole()`.

### Link To Affected Code

https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L310-L345

## [INFO-3] Create: `validateOrder()` Will Accept Offers Made From Rental Safes

This is just a heads up since this is technically out of scope. It requires `CREATE_SIGNER` signing a rental offer from a Rental Safe. 

If the `CREATE_SIGNER` signs offers made from a Rental Safe and the fulfiller is the safe owner, `Seaport` will still successfully process the order since it filters out the `tranferFrom` tx because the NFT is already in the rental safe. `Create.validateOrder()` will still be successful because it won't know it's supposed to be invalid. This might cause problems with `STORE.isRented`.

### Recommendation

Make sure `CREATE_SIGNER` doesn't sign offers made from rental safes.

### Link To Affected Code

https://github.com/re-nft/smart-contracts/blob/main/src/policies/Create.sol#L733
