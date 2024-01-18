## Cross-Chain Deployment Challenges Due to Solidity Version 0.8.20
The protocol's contracts, compiled with Solidity version 0.8.20, may face deployment and operational challenges on blockchain networks that are not compatible with the Ethereum Shanghai hard fork. This compatibility issue stems from the use of new opcodes introduced in the hard fork, specifically integrated into Solidity 0.8.20 and later versions.

According to the following link:

https://github.com/ethereum/solidity/releases/tag/v0.8.20

Solidity 0.8.20 incorporates changes aligned with the Ethereum Shanghai hard fork, including the introduction of new EVM opcodes like `PUSH0`. Contracts compiled with this version of Solidity might produce bytecode that is not compatible with L2 blockchain networks, ie. Polygon and Avalanche the protocol intends to begin with, that have not implemented these changes. This incompatibility arises because such networks do not recognize or properly execute these new opcodes.

The severity is significant for projects aiming for multi-chain deployment and operation, particularly on chains that are not updated with the latest Ethereum enhancements. The primary concern is the inability to deploy or correctly operate these contracts on non-Shanghai-compatible chains, limiting the reach and functionality of the project. This could also become a problem if different versions of Solidity are used to compile contracts for different chains. The differences in bytecode between versions can impact the deterministic nature of contract addresses, potentially breaking counter-factuality.

To ensure broader compatibility across various blockchain networks, consider downgrading the Solidity compiler version to 0.8.19 or earlier, which does not include Shanghai hard fork specific opcodes. This ensures compatibility with a wider range of blockchain networks.

## Enhancing Fairness in Escrow Payment Systems with Fixed Fee Structures
In the evolving landscape of blockchain-based rental agreements, the implementation of a fixed fee structure at the time of the agreement's initiation stands out as a significant advancement towards ensuring fairness and transparency. This approach effectively addresses the potential issue of variable fees impacting the final payout to parties involved in ERC20 token transactions, be it as part of the offer or a consideration. By caching the fee within the rental agreement itself, all parties benefit from a predictable and transparent fee system. This method not only protects recipients (lender or renter) from receiving less than expected due to post-agreement fee increases but also bolsters trust in the platform. Such a system, grounded in predictability and fairness, is crucial in maintaining the integrity and reliability of blockchain-based escrow services.

Here's where `fee` could change:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L380-L388

```solidity
    function setFee(uint256 feeNumerator) external onlyByProxy permissioned {
        // Cannot accept a fee numerator greater than 10000.
        if (feeNumerator > 10000) {
            revert Errors.PaymentEscrow_InvalidFeeNumerator();
        }

        // Set the fee.
        fee = feeNumerator;
    }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L88-L91

```solidity
    function _calculateFee(uint256 amount) internal view returns (uint256) {
        // Uses 10,000 as a denominator for the fee.
        return (amount * fee) / 10000;
    }
```
to affect the following instances in `_settlePayment()`:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L242-L248

```solidity
                if (fee != 0) {
                    // Calculate the new fee.
                    uint256 paymentFee = _calculateFee(paymentAmount);

                    // Adjust the payment amount by the fee.
                    paymentAmount -= paymentFee;
                }
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L254-L277

```solidity
                if (orderType.isPayOrder() && !isRentalOver) {
                    // Interaction: a PAY order which hasnt ended yet. Payout is pro-rata.
                    _settlePaymentProRata(
                        item.token,
                        paymentAmount, // @audit
                        lender,
                        renter,
                        elapsedTime,
                        totalTime
                    );
                }
                // If its a PAY order and the rental is over, or, if its a BASE order.
                else if (
                    (orderType.isPayOrder() && isRentalOver) || orderType.isBaseOrder()
                ) {
                    // Interaction: a pay order or base order which has ended. Payout is in full.
                    _settlePaymentInFull(
                        item.token,
                        paymentAmount, // @audit
                        item.settleTo,
                        lender,
                        renter
                    );
```
## Addressing Fairness in Early Termination of Rental Agreements
The existing model of pro-rata payment distribution in blockchain-based rental agreements, especially in the context of early termination, raises significant fairness concerns. This model, which calculates payments based on time elapsed and deducts fees upfront, can lead to disproportionately small compensations for renters, particularly in scenarios where early termination yields substantial benefits for the lender. 

Here's a possible scenario:

If the rental is 100 days and is stopped 1 hour of time elapsed because the renter has already performed a big lucky yield for the lender in the first hour. The renter ends up geting dust amount in compensation. Say fee is 10% and renter's compensation for 100 days is 10000 USDC. The renter ends up getting only 4 USDC vs a big profit gained to the lender in the NFT gaming.

Such situations highlight a critical need for a more equitable approach. Adjusting the pro-rata formula, revising the fee structure, and introducing specific early termination clauses could ensure a fairer distribution of benefits. This could involve setting a minimum guaranteed payment for the renter or adjusting the split based on the outcome of the rental (e.g., profit achieved). By implementing these changes, the system would not only uphold the principles of transparency and fairness but also maintain the integrity and trustworthiness of blockchain-based rental services.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L254-L265

```solidity
                // If its a PAY order but the rental hasn't ended yet.
                if (orderType.isPayOrder() && !isRentalOver) {
                    // Interaction: a PAY order which hasnt ended yet. Payout is pro-rata.
                    _settlePaymentProRata(
                        item.token,
                        paymentAmount,
                        lender,
                        renter,
                        elapsedTime,
                        totalTime
                    );
                }
```
## Streamlined Handling of PAYEE Orders in the Rental Protocol
In the `Create` contract of a blockchain-based rental system, the handling of PAYEE orders demonstrates a strategic and tailored approach. This contract, being a complex assembly within the rental protocol, showcases a nuanced understanding of PAYEE orders as mirror-images of PAY orders. Specifically, in the [_rentFromZone](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L546-L552) function, the invocation of [_convertToItems](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L401-L453) includes a unique process for PAYEE orders. Unlike BASE or PAY orders where item transformation is crucial, [PAYEE orders](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L360-L399) focus more on validation checks, reflecting their conceptual distinction. This design choice indicates that the primary objective for PAYEE orders is not to modify the [Item[] memory items](https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol#L415) array, but rather to ensure the correctness and integrity of the order through rigorous validations. This context-specific logic underlines the flexibility and adaptability of the contract in handling different order types, emphasizing a comprehensive approach to each order's unique characteristics. As such, the contract exemplifies a well-thought-out structure, catering to the diverse needs of a rental protocol while maintaining a robust and secure framework.

## Understanding Chain Reorganizations in Polygon and Avalanche
Chain reorganizations (reorgs) on Layer 2 solutions like Polygon and Avalanche (the protocol intends to begin with) have distinct characteristics owing to their unique consensus mechanisms. Polygon, leveraging a Proof of Stake (PoS) system, generally exhibits a lower probability of deep reorgs compared to Proof of Work networks. This is attributed to its PoS consensus providing more predictable finality and stability, though faster block times may lead to frequent but shallow reorgs. Validators play a pivotal role in maintaining network integrity on Polygon, influencing the occurrence and impact of reorgs. On the other hand, Avalanche stands out with its innovative consensus protocol that blends classical and Nakamoto consensus models. This approach ensures rapid transaction finality and a high throughput, significantly minimizing the chances of reorgs. The structure of Avalanche into multiple subnetworks further localizes the effect of reorgs to individual subnets, bolstered by their respective validators and staking mechanisms.

For both networks, developers are advised to design smart contracts that can gracefully handle potential reorgs, especially when dealing with time-sensitive operations or cross-chain functionalities. Continuous network monitoring and user education regarding transaction finality and reorg implications are also crucial. In essence, while Polygon and Avalanche offer improved stability against deep chain reorgs compared to Ethereum's mainnet, their unique architectures necessitate tailored considerations in smart contract development and deployment strategies.

## Typo and Comment Context Corrections
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L146

```diff
-            // If the stopper is the lender, then it doesnt matter whether the rental
+            // If the stopper is the lender, then it doesn't matter whether the rental
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L252

```diff
-     * @dev Modifier which only allows calls by an executing address.
+     * @dev Modifier which only allows calls by an executor address.
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L94

```diff
-     * @dev Safe transfer for ERC20 tokens that do not consistently renturn true/false.
+     * @dev Safe transfer for ERC20 tokens that do not consistently return true/false.
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol#L131

```diff
-     * @notice Fetches the hook address that is pointing at the the target.
+     * @notice Fetches the hook address that is pointing to the the target.
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Reclaimer.sol#L78

```diff
-        // initates the reclaim. In the context of a delegate call, address(this)
+        // initiates the reclaim. In the context of a delegate call, address(this)
```
https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol#L32

```diff
- * @notice Acts as an interface for all behavior related to stoping a rental.
+ * @notice Acts as an interface for all behavior related to stopping a rental.
```
## Private function with embedded modifier reduces contract size
Consider having the logic of a modifier embedded through a private function to reduce contract size if need be. A `private` visibility that is more efficient on function calls than the `internal` visibility is adopted because the modifier will only be making this call inside the contract.

For instance, the modifier inside the big Ocean contract may be refactored as follows:

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol#L77-L82

```diff
+    function _permissioned() private view {
+        if (!kernel.modulePermissions(KEYCODE(), Policy(msg.sender), msg.sig)) {
+           revert Errors.Module_PolicyNotAuthorized(msg.sender);
+        } 
+    }

      modifier permissioned() {
-        if (!kernel.modulePermissions(KEYCODE(), Policy(msg.sender), msg.sig)) {
-           revert Errors.Module_PolicyNotAuthorized(msg.sender);
-        } 
+        _permissioned();
      _;
      }
```
## Activate the Optimizer
Before deploying your contract, activate the optimizer when compiling using “solc --optimize --bin sourceFile.sol”. By default, the optimizer will optimize the contract assuming it is called 200 times across its lifetime. If you want the initial contract deployment to be cheaper and the later function executions to be more expensive, set it to “ --optimize-runs=1”. Conversely, if you expect many transactions and do not care for higher deployment cost and output size, set “--optimize-runs” to a high number.

```
module.exports = {
solidity: {
version: "0.8.20",
settings: {
 optimizer: {
   enabled: true,
   runs: 1000,
 },
},
},
};
```
Please visit the following site for further information:

https://docs.soliditylang.org/en/v0.5.4/using-the-compiler.html#using-the-commandline-compiler

Here's one example of instance on opcode comparison that delineates the gas saving mechanism:

```
for !=0 before optimization
PUSH1 0x00
DUP2
EQ
ISZERO
PUSH1 [cont offset]
JUMPI

after optimization
DUP1
PUSH1 [revert offset]
JUMPI
```
Disclaimer: There have been several bugs with security implications related to optimizations. For this reason, Solidity compiler optimizations are disabled by default, and it is unclear how many contracts in the wild actually use them. Therefore, it is unclear how well they are being tested and exercised. High-severity security issues due to optimization bugs have occurred in the past . A high-severity bug in the emscripten -generated solc-js compiler used by Truffle and Remix persisted until late 2018. The fix for this bug was not reported in the Solidity CHANGELOG. Another high-severity optimization bug resulting in incorrect bit shift results was patched in Solidity 0.5.6. Please measure the gas savings from optimizations, and carefully weigh them against the possibility of an optimization-related bug. Also, monitor the development and adoption of Solidity compiler optimizations to assess their maturity.