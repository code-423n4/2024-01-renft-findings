## [G-01] Using XOR (^) and AND (&) bitwise equivalents

Given 4 variables a, b, c and d represented as such:

    0 0 0 0 0 1 1 0 <- a
    0 1 1 0 0 1 1 0 <- b
    0 0 0 0 0 0 0 0 <- c
    1 1 1 1 1 1 1 1 <- d

To have a == b means that every 0 and 1 match on both variables. Meaning that a XOR (operator ^) would evaluate to 0 ((a ^ b) == 0), as it excludes by definition any equalities.Now, if a != b, this means that there’s at least somewhere a 1 and a 0 not matching between a and b, making (a ^ b) != 0.Both formulas are logically equivalent and using the XOR bitwise operator costs actually the same amount of gas.However, it is much cheaper to use the bitwise OR operator (|) than comparing the truthy or falsy values.These are logically equivalent too, as the OR bitwise operator (|) would result in a 1 somewhere if any value is not 0 between the XOR (^) statements, meaning if any XOR (^) statement verifies that its arguments are different.##    

There are 3 instances of this issue in 2 files:

```
File: src/policies/Create.sol	

311: if (totalRentals == 0 || totalPayments == 0) {
 
396: if (totalRentals == 0 || totalPayments == 0) {
```
    diff --git a/src/policies/Create.sol b/src/policies/Create.sol
    index 061180d..fc25e0d 100644
    --- a/src/policies/Create.sol
    +++ b/src/policies/Create.sol
    @@ -308,7 +308,7 @@ contract Create is Policy, Signer, Zone, Accumulator {
             }

             // PAY order offer must have at least one rental and one payment.
    -        if (totalRentals == 0 || totalPayments == 0) {
    +        if (((totalRentals ^ 0) & (totalPayments ^ 0)) == 0) {
                 revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments);
             }
         }
    @@ -393,7 +393,7 @@ contract Create is Policy, Signer, Zone, Accumulator {
             }

             // PAYEE order consideration must have at least one rental and one payment.
    -        if (totalRentals == 0 || totalPayments == 0) {
    +        if (((totalRentals ^ 0) & (totalPayments ^ 0)) == 0) {
                 revert Errors.CreatePolicy_ItemCountZero(totalRentals, totalPayments);
             }
         }

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/policies/Create.sol

```
File: src/Kernel.sol	

392: if (address(oldModule) == address(0) || oldModule == newModule_) {
```
    diff --git a/src/Kernel.sol b/src/Kernel.sol
    index 154d9d8..7af06f4 100644
    --- a/src/Kernel.sol
    +++ b/src/Kernel.sol
    @@ -389,7 +389,7 @@ contract Kernel {

             // Check that the old module contract exists, and that the old module
             // address is not the same as the new module
    -        if (address(oldModule) == address(0) || oldModule == newModule_) {
    +        if (((address(oldModule) ^ address(0)) & (oldModule ^ newModule_)) == 0) {
                 revert Errors.Kernel_InvalidModuleUpgrade(keycode);
             }

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/Kernel.sol

#### Test Code

    contract GasTest is DSTest {
        Contract0 c0;
        Contract1 c1;
        function setUp() public {
            c0 = new Contract0();
            c1 = new Contract1();
        }
        function testGas() public {
            c0.not_optimized(1,2);
            c1.optimized(1,2);
        }
    }

    contract Contract0 {
        function not_optimized(uint8 a,uint8 b) public returns(bool){
            return ((a==1) || (b==1));
        }
    }

    contract Contract1 {
        function optimized(uint8 a,uint8 b) public returns(bool){
            return ((a ^ 1) & (b ^ 1)) == 0;
        }
    }

#### Gas Report

| Contract0 contract                        |                 |     |        |     |         |
|-------------------------------------------|-----------------|-----|--------|-----|---------|
| Deployment Cost                           | Deployment Size |     |        |     |         |
| 46099                                     | 261             |     |        |     |         |
| Function Name                             | min             | avg | median | max | # calls |
| not_optimized                             | 456             | 456 | 456    | 456 | 1       |

| Contract1 contract                        |                 |     |        |     |         |
|-------------------------------------------|-----------------|-----|--------|-----|---------|
| Deployment Cost                           | Deployment Size |     |        |     |         |
| 42493                                     | 243             |     |        |     |         |
| Function Name                             | min             | avg | median | max | # calls |
| optimized                                 | 430             | 430 | 430    | 430 | 1       |

## [G-02] Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it’s cheaper to use the state variable directly that one time, and save the 3 gas the extra stack assignment would spend.

There is 1 instance of this issue in 1 file:

```
File: src/modules/PaymentEscrow.sol	

399: uint256 syncedBalance = balanceOf[token];
```
    diff --git a/src/modules/PaymentEscrow.sol b/src/modules/PaymentEscrow.sol
    index 34c4f4c..6426645 100644
    --- a/src/modules/PaymentEscrow.sol
    +++ b/src/modules/PaymentEscrow.sol
    @@ -395,14 +395,12 @@ contract PaymentEscrow is Proxiable, Module, PaymentEscrowBase {
          * @param to    Address to send the collected tokens.
          */
         function skim(address token, address to) external onlyByProxy permissioned {
    -        // Fetch the currently synced balance of the escrow.
    -        uint256 syncedBalance = balanceOf[token];

             // Fetch the true token balance of the escrow.
             uint256 trueBalance = IERC20(token).balanceOf(address(this));

             // Calculate the amount to skim.
    -        uint256 skimmedBalance = trueBalance - syncedBalance;
    +        uint256 skimmedBalance = trueBalance - balanceOf[token];

             // Send the difference to the specified address.
             _safeTransfer(token, to, skimmedBalance);

https://github.com/re-nft/smart-contracts/blob/3ddd32455a849c3c6dc3c3aad7a33a6c9b44c291/src/modules/PaymentEscrow.sol

#### Test Code

    contract GasTest is DSTest {
        Contract0 c0;
        Contract1 c1;
        function setUp() public {
            c0 = new Contract0();
            c1 = new Contract1();
        }
        function testGas() public {
            c0.not_optimized();
            c1.optimized();
        }
    }
    
    contract Contract0 {
        uint256 num = 5;
        uint256 sum;
        function not_optimized() public returns(bool){
            uint256 num1 = num;
            sum = num1 + 2;
        }
    }
    
    contract Contract1 {
        uint256 num = 5;
        uint256 sum;
        function optimized() public returns(bool){
            sum = num + 2;
        }
    }

#### Gas Report

| Contract0 contract                        |                 |       |        |       |         |
|-------------------------------------------|-----------------|-------|--------|-------|---------|
| Deployment Cost                           | Deployment Size |       |        |       |         |
| 63799                                     | 244             |       |        |       |         |
| Function Name                             | min             | avg   | median | max   | # calls |
| not_optimized                             | 24462           | 24462 | 24462  | 24462 | 1       |

| Contract1 contract                        |                 |       |        |       |         |
|-------------------------------------------|-----------------|-------|--------|-------|---------|
| Deployment Cost                           | Deployment Size |       |        |       |         |
| 63599                                     | 243             |       |        |       |         |
| Function Name                             | min             | avg   | median | max   | # calls |
| optimized                                 | 24460           | 24460 | 24460  | 24460 | 1       |