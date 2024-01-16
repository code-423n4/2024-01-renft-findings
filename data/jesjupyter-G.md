## There's no need to store `deployed[targetDeploymentAddress] = true` and check `if (deployed[targetDeploymentAddress])`

In the contract `Create2Deployer`, [a mapping `deployed` is used to prevent double deployment](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol#L45-L47).

```
        if (deployed[targetDeploymentAddress]) {
            revert Errors.Create2Deployer_AlreadyDeployed(targetDeploymentAddress, salt);
        }
```

However, this is redundant. As the `CREATE2` OpCode would return `address(0)` when the initial address has been occupied. Thus the transaction would revert due to `deploymentAddress != targetDeploymentAddress`. So some gas is wasted.