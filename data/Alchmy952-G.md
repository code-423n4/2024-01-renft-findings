Summary
Unchecked array length in addRentals functions

Proof of Concept
The code in this function iterates over arrays, the length of this array is not checked explicitly. Consider the code below:
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ArrayLengthAttack {
    // Mapping to store user balances
    mapping(address => uint256) public balances;

    // Function to deposit funds
    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    // Function with an unchecked array length
    function withdrawAllFunds(address[] calldata recipients) external {
        // This function has an unchecked array length, which can lead to gas exhaustion

        // Iterate over the array and send the entire balance to each recipient
        for (uint256 i = 0; i < recipients.length; i++) {
            // Potential gas exhaustion vulnerability due to the lack of array length check
            payable(recipients[i]).transfer(balances[msg.sender]);
        }
    }
}

Under the withdrawAllFunds function, there is no explicit check for the length of recipients

Impact
This can lead to an attacker developing malicious transactions with large arrays to prevent the function from executing by exhausting the gas
Link
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L189
Mitigation
A statement of the form below will remediate the issue:
require(rentalAssetUpdates.length <= MAX_ARRAY_LENGTH, "Array length exceeds maximum");

MAX_ARRAY_LENGTH is a constant that represents the maximum allowed length for the array
