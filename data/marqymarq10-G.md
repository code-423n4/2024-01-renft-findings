## Issue #1 : For Loop Optimization
Throughout the protocol, all for loops are written in the following manor:
```
for(uint56 i; i < array.length; i++) {
    // code
}
```
This is very gas inefficient for solidity. Because a for loop runs the code inside the declaration in each iteration, we are assigning `i`, checking `array.length`, and incrementing `i` on each iteration. 

It is more efficient to use a while loop and declare the variables outside of the loop, like so:
```
uint256 i;
uint256 length = array.length;

while (i < length) {
    // code
    unchecked {
        ++i;
    }
}
```

As mentioned above, we save gas by declaring `i` and `length` outside of the loop. Additionally, we save gas inside the loop by using `++i` and an unchecked block to skip checks on overflows and underflows on `i`'s incrementation. 

## Issue #2 : Declaring Memory Variables Outside of For Loops
Many for loops follow a similar flow to the code snippet below:
```
for(uint56 i; i < array.length; i++) {
    Struct memory myStruct = array[i];
    // code
}
```
This creates a new memory variable on each iteration. Memory in solidity becomes quadratically priced after the first 22 words. This quickly occurs gas fees as the loop has to assign a large portion of memory to these variables. 

It is cheaper to assign the memory variable outside of the for loop, and then set it to a new value inside the loop. Here is an example:
```
Struct memory myStruct;
for(uint56 i; i < array.length; i++) {
    myStruct = array[i];
    // code
}
```


## Issue #3 : Changing Memory Variables to Immutable Variables
Inside `Signer::_deriveRentalTypehashes()`, there are 6 hardcoded memory variables. Memory in solidity becomes quadratically priced after the first 22 words. Immutable variables, because they do not use storage slots, are cheaper in this instance to use. This saves on gas instead of writing to memory each time this function is called, you simply read from the smart contracts deployed code.