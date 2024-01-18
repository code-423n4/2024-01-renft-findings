## Contracts use solidity compiler version `0.8.20` which is currently not supported on L2's

The PUSH0 opcode was added to solidity in version `0.8.20` and is currently not supported on Polygon or Avalanche. Contract versions should be downgraded to `0.8.19`



## RentalOrder hashes do not include rentalWallet leading to possible hash collisions 

Rental orders are stored internally as hashes derived from the `_deriveRentalOrderHash()` method. This hash does not include the rentalWallet id. consider adding the rentalWallet id to the hash to ensure hash uniqueness

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol#L162-L195