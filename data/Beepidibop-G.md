# Gas Findings

## [GAS-1] `Kernel`: Update Index Before Pushing to an Array

`Kernel:_activatePolicy()` can have L431's `getPolicyIndex[policy_] = activePolicies.length - 1;` moved to before L428's `activePolicies.push(policy_);` and save some gas from the `-1` needed to correct the index.

The same thing can be done for the `for` loop in L441-L445.

```
before:
test_ActivatePolicy() (gas: 332204)

after:
test_ActivatePolicy() (gas: 331574)

Gas savings: 630
```

### Sample Gas Test:

```
// SPDX-License-Identifier: BUSL-1.1

pragma solidity ^0.8.20;

  

import {

    Actions,

    OrderType,

    OrderMetadata,

    RentalOrder

} from "@src/libraries/RentalStructs.sol";

import {

    StopUpgrade,

    StopPolicyMigration

} from "@src/examples/upgrades/StopPolicyUpgrade.sol";

  

import {BaseTest} from "@test/BaseTest.sol";

  

contract Kernel_Gas_Test is BaseTest {

    // The new policy to upgrade to

    StopUpgrade newStopPolicy;

  

    function setUp() public override {

        super.setUp();

        // deploy the new stop policy

        newStopPolicy = new StopUpgrade(kernel);

    }

  

    function test_ActivatePolicy() public {

        // impersonate the deployer which is the kernel admin

        vm.startPrank(deployer.addr);

  

        // enable the new stop policy

        kernel.executeAction(Actions.ActivatePolicy, address(newStopPolicy));

  

        // stop impersonating

        vm.stopPrank();

    }

}
```

### Improved Function:

```
    function _activatePolicy(Policy policy_) internal {

        // Ensure that the policy is not already active.

        if (policy_.isActive())

            revert Errors.Kernel_PolicyAlreadyApproved(address(policy_));

  

        // Grant permissions for policy to access restricted module functions.

        Permissions[] memory requests = policy_.requestPermissions();

        _setPolicyPermissions(policy_, requests, true);

  

        // Set the index of the policy in the array of active policies.

        getPolicyIndex[policy_] = activePolicies.length;

  

        // Add policy to list of active policies.

        activePolicies.push(policy_);

  

        // Fetch module dependencies.

        Keycode[] memory dependencies = policy_.configureDependencies();

        uint256 depLength = dependencies.length;

  

        // Loop through each keycode the policy depends on.

        for (uint256 i; i < depLength; ++i) {

            Keycode keycode = dependencies[i];

  

            // Set the index of the policy in the array of dependents.

            getDependentIndex[keycode][policy_] = moduleDependents[keycode].length;

  

            // Push the policy to the array of dependents for the keycode

            moduleDependents[keycode].push(policy_);

        }

  

        // Set policy status to active.

        policy_.setActiveStatus(true);

    }
```

### Link To Affected Code

https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L427-L431
https://github.com/re-nft/smart-contracts/blob/main/src/Kernel.sol#L441-L445
