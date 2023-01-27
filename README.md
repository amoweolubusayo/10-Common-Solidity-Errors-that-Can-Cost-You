# 10-Common-Solidity-Errors-that-Can-Cost-You

Solidity is a programming language used to write smart contracts on the Ethereum blockchain. Here are some of the most common Solidity errors developers make and code examples to help you understand and fix them.

## Reverting without an error message

One common error in Solidity is reverting a transaction without an error message. This can happen when the require() or assert() function is used without an error message.


```Solidity
require(msg.value >= 100);
``` 

## Fix

Provide an error message for the require() or assert() function.


```Solidity
require(msg.value >= 100, "You need to send at least 100 wei to execute this function.");
```

## Overflow/Underflow errors

Another common error in Solidity is an overflow or underflow error. This occurs when a value is too large or too small to be stored in a variable.


```Solidity
uint256 x = 2**256;
```

## Fix

Make sure that the value you are trying to store in a variable is within the range of that variable


```Solidity
uint256 x = 2**255;
```

## Uninitialized storage pointers

An uninitialized storage pointer error occurs when a storage variable is not assigned a value before it is used.


```Solidity
address public owner;
function transferOwnership(address newOwner) public { owner = newOwner; }
```

## Fix

Initialize the storage variable with a default value before using it.


```Solidity
address public owner = msg.sender;
function transferOwnership(address newOwner) public { owner = newOwner; }
```

## Unhandled exceptions

Unhandled exceptions occur when a function does not have a catch statement to handle errors.


```Solidity
function divide(uint a, uint b) public pure returns (uint) { 
return a / b; 
}
```

## Fix

Add a catch statement to handle errors.


```Solidity
function divide(uint a, uint b) public pure returns (uint) { 
require(b != 0, "Cannot divide by zero."); 
return a / b; 
}
```

## Out-of-gas errors

Out-of-gas errors occur when a smart contract runs out of gas during execution. This can happen when a contract has too many nested loops or if it performs too many computations.


```Solidity
function performComplexComputation() public { 
// Perform a lot of computations 
}
```

## Fix

Optimize your contract by reducing the number of computations, breaking large computations into smaller chunks, and removing unnecessary nested loops.


```Solidity
function performComplexComputation() public { 
// Divide the computations into smaller chunks 
uint256 chunks = computeChunks(); for (uint256 i = 0; i < chunks; i++) { 
performChunk(); 
} 
}
function computeChunks() internal view returns (uint256) { 
// Calculate the number of chunks needed for the computations 
return (data.length / CHUNK_SIZE) + 1; }

function performChunk() internal { 
// Perform a small portion of the computations 
}
```

## Unsafe low-level calls

Unsafe low-level calls occur when a contract calls another contract without properly checking its state. This can lead to reentrancy attacks.


```Solidity
function callAnotherContract() public { 
address targetContract = 0x123456789abcdef; 
targetContract.call.value(100)(); 
}
```

## Fix

Use the safeLowlevelCall() function or the address.call.value() with a gas limit to ensure that the other contract is in a safe state before calling it.


```Solidity
function callAnotherContract() public {
    address targetContract = 0x123456789abcdef;
    bytes memory data = "";
    (bool success, bytes memory result) = targetContract.safeLowlevelCall.value(100)(data);
    require(success, "The call to the target contract failed.");
}

//or

function callAnotherContract() public {
    address targetContract = 0x123456789abcdef;
    bytes memory data = "";
    targetContract.call.value(100)(data);
}
```

## Unchecked return values

Unchecked return values occur when a contract calls another contract and does not check the return value. This can lead to unexpected behavior.


```Solidity
function callAnotherContract() public { 
address targetContract = 0x123456789abcdef; 
targetContract.call(); 
}
```

## Fix

Check the return value of the other contract before continuing with the execution of your contract.


```Solidity
function callAnotherContract() public returns (bool) { address targetContract = 0x123456789abcdef; 
// call the other contract and check the return value 
bytes memory data = targetContract.call(); 
bool success = abi.decode(data, (bool)); 
require(success, "The call to the other contract failed."); 
return success; 
}
```

## Unbounded loops

Unbounded loops occur when a contract has a loop that can run indefinitely. This can lead to out-of-gas errors and cause the contract to crash.


```Solidity
function infiniteLoop() public { 
while(true) { 
// Perform some computation 
}}
```

## Fix

Add a break condition to the loop to ensure that it can only run for a certain number of iterations.


```Solidity
function infiniteLoop() public { // Set a maximum number of iterations 
uint256 maxIterations = 1000; 
// Initialize a counter 
uint256 i = 0; 
// Perform the loop
 while(i < maxIterations) { 
// Perform some computation 
i++; 
} 
}
```

## Unchecked return value before execution(very common with transfer)

One common error people make with the transfer() function in Solidity is not checking the return value before continuing the execution. This can lead to unexpected behavior and bugs in the contract, and it also makes it impossible to handle errors or revert the transaction if the transfer fails.


```Solidity
function transferFunds(address recipient, uint256 amount) public { recipient.transfer(amount); // Do some more processing }
```

## Fix 1

Ensure that the transfer of funds is successful and handle any errors


```Solidity
function transferFunds(address recipient, uint256 amount) public returns (bool) { 
bool success = recipient.call.value(amount)(""); 
require(success, "The transfer failed."); 
return success; 
}
```

## Fix 2

Use the transfer() or transferFrom() functions provided by the ERC20 standard, which will automatically revert the transaction in case of failure.


```Solidity
function transferFunds(address recipient, uint256 amount) public returns (bool) { 
require(address(this).transfer(recipient, amount), "The transfer failed."); 
return true; 
}
```

## Incorrect function visibility

Incorrect function visibility occurs when a contract's functions are not marked with the proper visibility (e.g. public, private, internal) which can lead to unexpected behavior and security vulnerabilities.


```Solidity
function sensitiveFunction() { 
// Perform sensitive operations 
}
```

## Fix

```Solidity
function sensitiveFunction() private { 
// Perform sensitive operations 
}
```

These are just a few common errors developers make while writing smart contracts, and there are many other potential errors that can occur in Solidity. To avoid them, thoroughly test and review your smart contract code, so you can help to ensure that it is robust and 'secure'.
