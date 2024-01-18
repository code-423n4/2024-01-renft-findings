### Description:
reNft can be described as a collateral-free NFT rental Protocol that rents in game NFTs. without collateral from the renter. It is a system where anyone can be able list their in game NFTs for rent and then a renter comes along and fulfils the rent order. 
   
**Existing Standards:**
- The protocol adheres to conventional Solidity and Ethereum practices, primarily utilizing the ERC20 standards,ERC721 along with the Gnosis Safe integration used in deploying Rental safes used to store Rental assets.


It's worth noting that the protocol relies on Gnosis Safe for the deployments of rental safes

## 1- Approach:
- Scope: I initiated the process by evaluating the extent of the code, which then directed my method for dissecting the project.

- Top-down approach: 
```
1.Create.sol
2.Stop.sol
3.Guard.sol
4.Factory.sol
5.Kernel.sol
6.PaymentEscrow.sol
7.Storage.sol
8.Accumulator.sol
9.Reclaimer.sol
```


## 2-  Codebase analysis:
In my assessment, the quality of the codebase is currently satisfactory, with measures in position to manage diverse roles and ensure adherence to established standards.

| Codebase Quality Categories  | Comments |
| --- | --- |
| **Code Comments**  | Sufficient overall code comments, Just noticed some little misspellings in some code comments although it wasn't much.  |
| **Documentation** | Overall protocol documentation is robust in explaining the intent of the protocol as the protocol is not so Big with complexity that will require much documentation. |
| **Testing** | The Protocol did well by providing test suites which made it easier to get a grasp on different functionalities implemented in the codebase. With a fully functional test at my disposal, I could test intricate concepts and potential vulnerability leads.  |

## 3- Threat Modelling:
After running some forge tests, I began formulating specific assumptions that if compromised, could pose security risks to the system. This process aids me in determining the most effective exploitation strategies. Thoroughly examining and marking any doubtful or vulnerable areas within the protocol, diving deep into these areas and performing in-depth examinations and subjecting them to rigorous testing.
I also went through the findings from the bot race in order to not report already reported issues again, while bearing in mind invariats and critical parts of the functions. 

## 4-  Centralization Risks:
None. 

## 5-  Architecture & Recommendations:
This Protocol supports a well planned implementation,and as the team understands this, I must commend the considered various security measures and roles to mitigate potential risks.

I would love to add something I noticed in the `PaymentEscrow.sol` contract. Its just little fix but will be contributing to the overall security of the `reNFT` protocol.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol#L100C3-L117C8

In the above function a low-level call is made and its advisable to check if the receiver is a contract when making such calls.
  
### Conclusion:

In summary, I believe that the `reNft` demonstrates promise as a collateral-free NFT rental Protocol. 

### Time spent:

21 hours



### Time spent:
21 hours