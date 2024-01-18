## [N-01] Typos

There are 9 instances of this issue in 5 files:

    File: src/modules/PaymentEscrow.sol	

    /// @audit renturn
    94: * @dev Safe transfer for ERC20 tokens that do not consistently renturn true/false.

    /// @audit amoutn
    130: * @return lenderAmount Payment amoutn to send to the lender.

    ///  @audit accoutn
    155: * @param renter      Renter accoutn.

    /// @audit ordesr
    335: * @param orders Rental ordesr for which to settle payments.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

    File: src/modules/Storage.sol	

    /// @audit addresss
    269: * @notice Adds the addresss of a rental safe to storage so that protocol-deployed

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/Storage.sol

    File: src/packages/Signer.sol	

    /// @audit fulfilmment
    207: // Derive and return the fulfilmment hash as specified by EIP-712

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/packages/Signer.sol

    File: src/policies/Stop.sol	

    /// @audit stoping
    32: * @notice Acts as an interface for all behavior related to stoping a rental.

    /// @audit Wmit
    112: // Wmit the event.

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Stop.sol

    File: src/Create2Deployer.sol	

    /// @audit sendder
    99: *         This function ties the deployment sendder to the salt of the CREATE2

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Create2Deployer.sol


## [N-02] According to the syntax rules, use *=> mapping (* instead of *=> mapping(* using spaces as keyword

There are 3 instances of this issue in 1 file:

    File: src/Kernel.sol	

    218: mapping(Keycode => mapping(Policy => uint256)) public getDependentIndex;

    221: mapping(Keycode => mapping(Policy => mapping(bytes4 => bool)))

    229: mapping(address => mapping(Role => bool)) public hasRole;

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

## [N-03] Empty/Unused Function Parameters

Empty or unused function parameters should be commented out as a better and declarative way to silence runtime warning messages. 

There is 1 instance of this issue in 1 file:

As an example, the following function may have these parameters refactored to:

    File: src/policies/Guard.sol	

    309: function checkTransaction(
    310:     address to,
    311:     uint256 value,
    312:     bytes memory data,
    313:     Enum.Operation operation,
    314:     uint256 /* data1 */,
    315:     uint256 /* data2 */,
    316:     uint256 /* data3 */,
    317:     address /* data4 */,
    318:     address payable /* data5 */,
    319:     bytes memory /* data6 */,
    320:     address /* data7 */
    321: ) external override {

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Guard.sol