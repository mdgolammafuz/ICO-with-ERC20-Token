# Mastering-Solidity-Programming-from-Scratch

Fundamentals of **Solidity** for **Ethereum Blockchain**

<br/>

## Smart Contract Compilation
<br/>

![](assets/Screenshot137.png)

<br/>

* The Solidity source code is passed to the solidity compiler and the compile returns the
EVM bytecode that is deployed and the contract **ABI - Abstract Binary Interface**.
<br/>

* There are many solidity compilers available: Remix built-in compiler, solc, solcjs
<br/>

* Contract bytecode is public. It is saved on the Blockchain and can’t be encrypted because it must be run by every Ethereum node.
<br/>

* **Opcodes** are the human readable instructions of the program. They can be easily obtained from bytecode.
<br/>

* Contract source code doesn’t have to be public. Most contracts are public to build trust.
<br/>

* Anyone that wants to interact with the contract must have access to the contract ABI.
ABI is basically how you call functions in a contract and get data back.
<br/>

* ABI is list of contract’s function and arguments and it’s in JSON format. ABI is known at compile time.
<br/>

* ABI is generated from source code through compilation. If we don’t have the source code we can’t generate the contract ABI (or only from the bytecode using reverse engineering).
 
<br/>

## Solidity Data Types
<br/>

Solidity is a statically-typed language (variables type should be specified at declaration). Data types are broken up into two main categories in Solidity:

* **Value types**

* **Reference types**
 
<br/>

### Value Types
<br/>

1. **Booleans**
In Solidity, `bool` stands for a Boolean value that is either `true` or `false`. You can do logical operations like “or,” “and,” “equal to,” “not equal to,” and so on, to get a Boolean value.
<br/>

* Boolean variables: `true` and `false`
<br/>

* Initialized by default with `false`
<br/>

2. **Integers**
<br/>

* Signed and Unsigned Integers of various sizes
<br/>

* int8 to `int256`, `uint8` to `uint256` in steps of 8
<br/>

* `int8` is between -128 and +127, int16 is between -32768 and +32767 and so on
<br/>

* `int` is alias to `int256` and `uint` is an alias to `uint256`
<br/>

* By default an int is initialized with zero
<br/>

* There is no full support for float/double (fixed point numbers) in Solidity
<br/>

3. **adress**
<br/>

While all the previously data types are common to other programming languages, the `address` data type is unique to Solidity. It's a 20-byte value that's used to represent an `address` on the Ethereum network. The address can be a user's Ethereum account or a contract deployed to the network. It has a `.balance()` **member** that can be called to check the balance associated with the account, and a `.transfer()` **member** that can be used to transfer funds to that address. There's also a `.send()` **member**, which is the low-level counterpart of the `.transfer()` **member**. If you use `.send()`, you must check the `return` value of the operation for success or failure, but if you're using .`transfer()`, the `transfer` member handles this for you automatically.

<br/>

> Please read about [SafeMath, Overflows and Underflows](https://peckshield.medium.com/alert-new-batchoverflow-bug-in-multiple-erc20-smart-contracts-cve-2018-10299-511067db6536).

<br/>

### Reference types

1. **Solidity Arrays**

* **Fixed-Size Arrays**
<br/>

* Has a compile-time fixed size.
<br/>

* Can store any type (`int`, `uint`, `address`, `struct` etc)
<br/>

* bytes1, bytes2, …, bytes32 store a sequence of bytes.
<br/>

* Has **member** called `length`
 
<br/>

<br/>

> **Coding: Fixed-Size Arrays**

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract FixedSizeArrays{
// declaring a fixed-size array of type uint with 3 elements
uint[3] public numbers = [2, 3, 4];
 
// declaring fixed-size arrays of type bytes
bytes1 public b1;
bytes2 public b2;
bytes3 public b3;
//.. up to bytes32
 
// changing an element of the array at a specific index
function setElement(uint index, uint value) public{
numbers[index] = value;
}
 
// returning the number of elements in the array
function getLength() public view returns(uint){
return numbers.length;
}
 
// setting bytes arrays
function setBytesArray() public{
b1 = 'a'; // => 0x61 (ASCII code of `a` in hex)
b2 = 'ab'; // => 0x6162
b3 = 'z'; // => 0x7A
// b3[0] = 'a'; // ERROR => can not change individual bytes
 
// byte is an alias for bytes1 on older code
}
}
 
```
<br/>

* **Dynamically-sized arrays**
<br/>

* `byte[ ]`
<br/>
  
* `byte[ ]` is an alias to `bytes`
<br/>

* `string` (UTF-8 encoding)
<br/>

* `uint[]`, `int[ ]`
<br/>

* **members**: `length` and `push`

> **Coding - Dynamic Arrays**

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract DynamicArrays{
// dynamic array of type uint
uint[] public numbers;
 
// returning length
function getLength() public view returns(uint){
return numbers.length;
}
 
// appending a new element
function addElement(uint item) public{
numbers.push(item);
}
 
// returning an element at an index
function getElement(uint i) public view returns(uint){
if(i < numbers.length){
return numbers[i];
}
return 0;
}
 
 
// removing the last element of the array
function popElement() public{
numbers.pop();
}
 
function f() public{
// declaring a memory dynamic array
// it's not possible to resize memory arrays (push() and pop() are not available).
uint[] memory y = new uint[](3);
y[0] = 10;
y[1] = 20;
y[2] = 30;
numbers = y;
}
 
}
```
<br/>


2. **Strings**
   
In Solidity, a string is a type passed by reference. The string data type is used for arbitrary-length UTF-8 and also costs more gas when compared to the fixed-size types of bytes1 to bytes32.

<br/>


> **String and Bytes**
> String is equal to bytes but does not allow `length` or `index` access.
<br/>

3. ***Structs and Enums***

* **Structs**
* A struct is a collection of key->value pairs;
* A struct introduces a new complex data type, that is composed of elementary data types
* Structs are used to represent a singular thing that has properties such as a Car, a Person, a Request and so on and we use mappings to represent a collection of things like a collection of Cars, Requests etc
* A struct is saved in storage and if declared inside a function it references storage by default

* **Enums**
* Enums are used to create user-defined types
<br/>

* Enums are explicitly convertible to and from integer
* Enums are user defined types that contain human readable names for a set of constants, called members

<br/>

> **Struct and Enums Coding**
<br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
// declaring a struct type outsite of a contract
// can be used in any contract declard in this file
struct Instructor{
    uint age;
    string name;
    address addr;
}
 
contract Academy{
    // declaring a state variabla of type Instructor
    Instructor public academyInstructor;
    
    // declaring a new enum type
    enum State {Open, Closed, Unknown}
    
    // declaring and initializing a new state variable of type State
    State public academyState = State.Open;
    
    // initializing the struct in the constructor
    constructor(uint _age, string memory _name){
        academyInstructor.age = _age;
        academyInstructor.name = _name;
        academyInstructor.addr = msg.sender;
    }
    
    // changing a struct state variable
    function changeInstructor(uint _age, string memory _name, address _addr) public{
        if (academyState == State.Open){
            Instructor memory myInstructor = Instructor({
                age: _age,
                name: _name,
                addr: _addr
            }
                );
            academyInstructor = myInstructor;
        }
    }
}
 
 
// the struct can be used in any contract declared in this file
contract School{
    Instructor public schoolInstructor;
} 
```

<br/>

4. **Mappings**
<br/>

* It’s a data structure that holds key->value pairs. Its similar to Python Dictionaries, JS objects or Java HashMaps
<br/>

* All keys must have the same type and all values must have the same type
   
* The keys can not be of types mapping, dynamic array, enum or struct. The values can be of any type including mapping
* Mapping is always saved in storage, its’ a state variable. Mappings declared inside functions are also saved in storage
* The mappings advantage is that lookup time is constant no matter mapping’s size
* A mapping is not iterable, we can’t iterate through a mapping using a for loop
* Keys are not saved into the mapping (its a hash table data structure). To get a value from the mapping we provide a key, the key gets passed through a hashing function, that outputs a predetermined index that returns the corresponding value from the mapping
* If we want the value of an unexisting key we get a default value

<br/>

> **Coding - Mappings**
> 
<br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
contract Auction{
    
    // declaring a variable of type mapping
    // keys are of type address and values of type uint
    mapping(address => uint) public bids;
    
    // initializing the mapping variable
    // the key is the address of the account that calles the function
    // and the value the value of wei sent when calling the function
    function bid() payable public{
        bids[msg.sender] = msg.value;
    }
}
```

> **Knowledge check: Solidity data types**

> 1. What is the correct way of defining a mapping?
> 
```
mapping(address => uint) public bids;

```

<br/>

> 2. What is the default value for a bool variable?

<br/>

**Answer: `false`** 

<br/>

> 3. Consider the following variable declarations:

<br/>

```
bytes public b1 = 'abc';
string public s1 = 'abc';
 ```
 What can you say about the above declarations?
<br/>

**Answer: We can add a new element to `b1` by calling the `push()` member function, but we can't do the same for `s1`. We can return a element of `b1` using indexing which is not possible for `s1`** 

<br/>

> 4. Consider the following struct type declaration:

<br/>

```
struct Auction{
    uint value;
    string description;
    address addr;
}
```
<br/>

How can you declare a state variable of type Auction?


<br/>

**Answer: `Auction myAuction`** 

<br/>

> 5. Consider the following Smart Contract. What will be the values of numbers after calling `f()`?

<br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract A{
    uint[] public numbers = [10, 20];
    
 
    function addElement(uint item) public{
        numbers.push(item);
    }
    
   
    function popElement() public{
        numbers.pop();
    }
    
    function f() public{
      numbers.pop();
      numbers.push(100);
      numbers[1] = 66;
    }
    
}
```
<br/>

<br/>

**Answer: `[10,66]`** 

<br/>

## Solidity Variables

<br/>

### Variable Types

<br/>

1. **State Variables**

* Declared at contract level
<br/>

* Permanently stored in contract storage
* Can be set as constants
* Expensive to use, they cost gas
* Initialized at declaration, using a constructor or after contract deployment by calling setters
<br/>

2. **Local variables**

* Declared inside functions
* If using the **memory** keyword and are arrays or struct, they are allocated at runtime.
* Memory keyword can’t be used at contract level
<br/>

> **Where does EVM save data?**

a) **Storage**

* Holds state variables
* Persistent and expensive (it costs gas)
* Like a computer HDD
<br/>

b) **Stack**

* Holds local variables defined inside functions if they are not reference types (ex: int)
* Free to be used (it doesn’t cost gas)
<br/>

c) **Memory**

* Holds local variables defined inside functions if they are reference types but declared with the **memory** keyword
* Holds function arguments
* Like a computer RAM
* Free to be used (it doesn’t cost gas)
<br/>

> Check the following links for more on [Storage, Memory and the Stack](https://docs.soliditylang.org/en/develop/introduction-to-smart-contracts.html?highlight=stack#storage-memory-and-the-stack) and [Ethereum: Datastore types explained](https://medium.com/@eiki1212/ethereum-datastore-types-explained-b085bc79aa4b)

<br/>

* Demonstration: How `memory` and `storage` work
<br/>

```
//SPDX-License-Identifier: GPL-3.0

    pragma solidity >= 0.5.0. < 0.9.0 ;

    contract A{
    string[] public cities = ['Mumbai','Delhi'];

    function f_memory() public{
        string[] memory s1 = cities ;
        s1[0] = 'Kolkata';
    }

    function f_storage() public{
        string[] storage s1 = cities ;
        s1[0] = 'Kolkata';
    }
}

```
<br/>

1. The state variable `cities` will not be modified, since the first element of the dynamic array has remain unchanged. So the function ``f_memory`` worked on a copy not, not on the **state** variable.
<br/>
 
![](assets/Screenshot205.png)

<br/>

2. In the above code, `s1` is the reference to the same memory location where the **state** variable `cities` was saved. Although, we didn't touch **state** variable `cities` inside the function `f_storage` ( we have only changed `s1` which is defined inside the function `f_storage`), changing `s1` also changes the **state** variable.
<br/>

![](assets/Screenshot205.png)

<br/>

> **Knowledge check: `memory` and `srorage`**

> 1. Consider the following contract. What will be the value of the crypto dynamic array after calling `myFunction()?`
> 
```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract A{
    string[] public crypto= ['BTC', 'ETH', 'BNB'];
    
    function myFunction() public{
        string[] memory s = crypto;
        s[2] = 'XMR';
    }
}

```
<br/>

**Answer: ['BTC', 'ETH', 'BNB']** 

<br/>

> 2. Consider the following contract. If you call `myFunction()` will the Blockchain state be altered?

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract A{
    string[] public crypto= ['BTC', 'ETH', 'BNB'];
    
    function myFunction() public{
        string[] storage s = crypto;
        s[2] = 'XMR';
    }
}
```
<br/>

**Answer: Yes** 

<br/>

> **Reference Types**: **string, array, struct and mapping**
<br/>

<br/>

## Variable scope

This brings us to a really interesting point: something called variable scope. Take a look at this. We have a variable called `saySomething` with a value of `"hello"`. Inside this `doStuff` function, we have another variable called saySomething with a value of `"goodbye"`. So, while we're inside this function, what do you think the value of `saySomething` is? If you said `goodbye`, you're right, the saySomething variable inside the function is said to be shadowing the same variable name outside the function, and as you can see that's a bad thing. When this function exits, the value of `saySomething` is now back to the original value of `"hello"`; that's because the variables declared inside this function only exist within the function. Once the function exits, those variables are gone. on exits, the value of `saySomething` is now back to the original value of `"hello"`; that's because the variables declared inside this function only exist within the function. Once the function exits, those variables are gone. Outside of the doStuff func there's not even a thing called `saySomethingElse` that can be accessed. That's an important point to remember when you're building your functions: what variables are needed inside the function and what data is needed from the function after it exits:

```
string saySomething = "hello";
function doStuff() internal {
         string saySomething = "goodbye";
         string saySomething = "I have nothing else to say";
}

//saySomething is "hello"
//saySomethingElse doesn't exist
```
<br/>

> **Coding - Variables and Functions with Basic Structure of Smart Contracts: Setters, Getters and the Constructor**
 
 <br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract Property{
    // declaring state variables saved in contract's storage
    uint price; // by default is private
    string public location;
    
    // can be initialized at declaration or in the constructor only
    address immutable public owner;
    
    // declaring a constant
    int constant area = 100;
    
    // declaring the constructor
    // is executed only once at contract's deployment
    constructor(uint _price, string memory _location){
        price = _price;
        location = _location;
        owner = msg.sender;  // initializing owner to the account's address that deploys the contract
    }
    
    
    // getter function, returns a state variable
    // a function declared `view` does not alter the blockchain 
    function getPrice() public view returns(uint){
        return price;
    }
    
    // setter function, sets a state variable
    function setPrice(uint _price) public{
        int a; // local variable saved on stack
        a = 10;
        price = _price;
    }
    
    function setLocation(string memory _location) public{ //string types must be declared memory or storage
        location = _location;
    }
    
}
```
<br/>

## Built-in Global Variables
<br/>

1. `msg` -> contains information about the account that generates the transaction and about 
the transaction or call, as well

    * `msg.sender` -> account address that creates and sends the transaction
    <br/>

    * `msg.value` -> ETH value (represented in wei) sent to this contract

    * `msg.data` -> data field in the transaction or call that called the function

2. `this` -> the current contract, explicitly convertible to Address. `(address)this.balance` returns the contract balance
3. `gasleft()` -> returns the remaining gas
4. `block.timestamp` -> returns the total running seconds since the Unix Epoch
5. `block.number`
6. `block.difficulty`
7. `block.gaslimit`
8. `tx.gasprice` -> gas price of the transaction

<br/>

## Contract Address
<br/>

  1. Any contract has its own unique address that is generated at deployment
<br/>

  1. The contract address is generated based on the address of the account that deploys the contract and the no. of transactions of that account (nonce . It can’t be calculated in advance.
<br/>

1. Address is a variable type and has the following members:
   
    * `balance`
    <br/>

    * If the address is declared payable it has two additional members:
        * `transfer()`: should be used in most cases as it's the safest way to send ether
        <br/>

        * `send()`: is like a low-level transfer(). If execution fails the contract will not stop and send() returns false
       
    * `call()`, `delegatecall()`, `staticcall()`

## Payable functions and contract balance

* A smart contract can receive ETH and can have an ETH balance only if there’s at least one payable function
<br/>

* A contract receives ETH in multiple ways:
  
* Just by sending ETH to the contract address from another account
<br/>

* `receive() external payable` - for empty calldata (and any value)
<br/>

* `fallback() external payable` - when no other function matches (not even the receive function).
<br/>

* By calling a payable function and sending ETH with that transaction
<br/>

* The ETH balance of the contract is in possession of anyone who can call the `transfer()` built-in function

<br/>

> **Demonstration: Reiceiving Ether**

<br/>

1. Let us demonstrate how to send ether to the contract's address using **Metamask** and **Remix** on **Rinkeby** **testnet** using the following **solidity** code given as a screenshot from the **Visual Studio Code Editor**
 
<br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.6.0 <0.9.0;
 
contract Deposit{
    
    // either receive() or fallback() is mandatory for the contract to receive ETH by 
    // sending ETH to the contract's address
    
    // declaring the receive() function that is executed when sending ETH to the contract address
    // it was introduced in Solidity 0.6 and a contract can have only one receive function, 
    // declared with this syntax (without the function keyword and without arguments). 
    receive() external payable{
    }
   
 
    /* declaring a fallback payable function that is called when msg.data is not empty or when no other function matches */
    fallback() external payable {
    }
    
    
    // ether can be send and received by the contract in the trasaction that calls this function as well
    function sendEther() public payable{
        
    }
 
   
    // returning the balance of the contract. 
    function getBalance() public view returns (uint) {
        // this is the current contract, explicitly convertible to address, 
        // and balance is a member of any variable of type address. 
        return address(this).balance;
    }
}
```

<br/>

![Solidity Code](assets/Screenshot182.png)

<br/>

2. Choose **Injected Web3** on **Remix** and deploy the contract. As soon as we deploy the contract on **Remix** , **Metamask** will pop up. Confirm the transaction on Metamask.
<br/>

![](assets/Screenshot170.png)

<br/>

![](Screenshot171.png)

<br/>

![](assets/Screenshot172.png)

<br/>

3. Once the **transaction** is **mined** on **Rinkeby** test network, a confirmation message will pop up from **Metamask**. We can see the details of the transaction on **Remix** as well as on **etherscan**.  
<br/>

![](assets/Screenshot173.png)

<br/>

4. Now, check the balance by calling the `getBalance()` function and note the balance.
 <br/>

![](assets/Screenshot174.png)

<br/>

5. Next, we send **55555** Wei to the contact address. As soon as we call `sendEther()` on Remix, **Metamask** will popup. Confirm the transaction on **Metamask**.
 <br/>

![](assets/Screenshot175.png)

<br/>

![](assets/Screenshot176.png)

<br/>

6. Once the transaction is mined, Metamask will show the confirmation message. Now, call `getBalance` again and check the balance to find it whether it is **55555 Wei** or not.
<br/>

![](assets/Screenshot177.png)

<br/>

![](assets/Screenshot184.png)

<br/>

## Securing the Contract's Balance against unauthorized access

One approach is to restrict the access of `transferEther()` to the account which deploys the contract to secure the Contract's balance against the unauthorized access. 
<br/>

> **Coding**

<br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 

 contract Deposit{
     address public owner;
     
     
    /* declaring the constructor to initialize the owner with 
    the address of the account that deploys the contract */

     constructor(){
        owner = msg.sender;    
     }
     
     receive() external payable{
     }
     
     
     function getBalance() public view returns(uint){
         return address(this).balance;
     }
     
 
    // transfering ether from the contract to another address (recipient)
     function transferEther(address payable recipient, uint amount) public returns(bool){
         // checking that only contract owner can send ether from the contract to another address
         require(owner == msg.sender, "Transfer failed, you are not the owner!!");
         
         if (amount <= getBalance()){
             // transfering the amount of wei from the contract to the recipient address
             // anyone who can call this function have access to the contract's funds
             recipient.transfer(amount);
             return true;
         }else{
             return false;
         }
     }     
 }
```

<br/>

**Demonstration**

<br/>

1. Let me choose the **first account** of my Metamask Wallet to deploy the contract, which means the **first account** is the **owner** of the **contract** which has the sole authority to access the `transferEther()`.
<br/>
    
![](assets/Screenshot185.png)

<br/>

![](assets/Screenshot186.png)

<br/>

2. Now, let me choose my second account of my Metamask Wallet, which is not the **owner** to access the `transferEther()`. It shows error as can be seen from the following screenshots!
<br/>

![](assets/Screenshot190.png)

<br/>

![](assets/Screenshot191.png)

<br/>

3. Finally, I choose the the **first account** of my Metamask Wallet to call `transferEther()`. This time, the transaction gets successfully mined into the **Rinkeby testnet**.
<br/>

![](assets/Screenshot192.png)

<br/>

![](assets/Screenshot194.png)

<br/>

![](assets/Screenshot195.png)

<br/>

## Variables and Functions Visibility: Private, Public, Internal, External
<br/>

There are four visibility specifiers for functions and state variables
<br/>

1. **Public**

* The function is part of the contract interface and can be called both **internally** (from within the same contract) and **externally** (from other contracts or by EOA accounts)
<br/>

* A **getter** is automatically created for **public** variables. They can be easily accessed from **dApps**

<br/>

2. **Private**

* **Private** functions and variables are available only in the contract they are defined 
* This is the **default** for **state variables**, they can be **accessed** only by a **getter** function

<br/>

3. **Internal**

* Functions are accessible only from the contract they are defined in and from derived contracts

<br/>

4. **External**

* The function is part of the contract interface, can be accessed only from other contracts or by EOA accounts using transactions. It’s automatically public

* Not-available for state variables

<br/>

> * Everything that is inside a contract is visible to all external observers. Making something private only prevents other contracts from accessing and modifying the information, but it will still be visible to the whole world outside of the blockchain
> 
> * Information is not encrypted by default on the Ethereum Blockchain
> 

> * The visibility specifier is given after the type for state variables and between parameter list and 
return parameter list for functions
>
> * External functions are more efficient than public functions in terms of gas consumption

<br/>

> **Coding: Visibility Specifiers**

<br/>

```
//SPDX-License-Identifier: GPL-3.0
 
pragma solidity >=0.5.0 <0.9.0;
 
contract A{
    int public x = 10;
    int y = 20; // internal by default
    
    function get_y() public view returns(int){
        return y;
    }
    
    function f1() private view returns(int){
        return x;
    }
    
    function f2() public view returns(int){
        int a;
        a = f1();
        return a;
    }
    
    function f3() internal view returns(int){
        return x;
    }
    
    function f4() external view returns(int){
        return x;
    }
    
    function f5() public pure returns(int){
        int b;
        // b = f4(); //f4() is external
        // Even though f4() wasn’t external it wouldn’t be possible to be called 
        // from within f5() because f5() is pure (it can not read nor write to the blockchain)
 
        return b;
    }
}
 
contract B is A{
    int public xx = f3();
    // int public yy = f1(); -> f1() is private and cannot be called from derived contracts
}
 
contract C{
    A public contract_a = new A();
    int public xx = contract_a.f4();
    // int public y = contract_a.f1();
    // int public yy = contract_a.f3();
}

```

<br/>

> **Demonstration**

<br/>

1. In the above code `y` is an **internal variable** that can be accessed both  in the **current** and **derived** contracts by a **getter** function. Please note that the **getter** function is automatically crreated for the `public` variable `x` but not for the `internal` one. If we want to access the `internal` variable `y` we have t use a **getter** function. `gety ()` is a `public` function and can be accessed can be called from the same contract or from external contracts or **applications** like **Remix**.
<br/>

![](assets/Screenshot196.png)

<br/>

2. `f1()` is declared to be an `private` function, so **Remix** is unable to call `f1()`. But the `private` function `f1()` can be called by another function like `f2()` in the same contract.
<br/>

![](assets/Screenshot197.png)

<br/>

3. `f3()` is an `internal` function which can can be called from the same contract ( e.g. **contract A**) as well as from derived contract ( e.g. **contract B** ). **Contract B** has been derived from **contrcat A**, same as the **class** is derived from another **class** in **obeject oriented programming**. **Contrcat B** has **inherited** all the functions and variables of **contrct A**. 
<br/>

![](assets/Screenshot198.png)

<br/>

4. Difference** between `private` and `internal` function: 
<br/>
A `private` function `f2()` can't be accesed from **derived contract B**, whereas an `internal` function `f3()` can be acccesed from **derived contract B**.
<br/>

![](assets/Screenshot199.png)

<br/>

5. We get an error if we want to access `external` function `f4()` in the same **contract A** inside function `f5()`. Note that we can access `f5()` though on **Remix** as **Remix** is an external application in this specific instance.
<br/>

![](assets/Screenshot200.png)

<br/>

6. In the above example, **contract C** is an **external contract** which deploys **contract A**. **Contract C** can access `external` function `f4()` but can't access either the `internal` function `f3()` or the `private` function `f2()`.
<br/>

![](assets/Screenshot201.png)

<br/>

![](assets/Screenshot202.png)

<br/>

![](assets/Screenshot203.png)

<br/>

> **Knowledge check: Visibility Specifier**

> 1. What visibility specifier has the `price` variable?
> 
```
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.5.0 <0.9.0;
 
contract Property{
    uint price;
    constructor(uint _price){
        price = _price;
    }
}
```
<br/>

**Answer: Internal** 

<br/>

> 2. Where can the `setPrice()` function be called from?

```
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.5.0 <0.9.0;
 
contract Property{
    uint  price;
    function setPrice(uint _price) private {
        price = _price;
    }
}
```
<br/>

**Answer: Only from the current comntract** 

<br/>

# Implementing an ERC 20 Token

## Contract Inheritance in Solidity 

<br/>

1. A contract acts like a **class**. A contract can **inherit** from another **contract** known as the **base contract** to share a common interface

<br/>

2. The general inheritance system is very similar to Python’s one, especially concerning **multiple inheritance**

3. Solidity supports multiple inheritance including polymorphism. Multiple inheritance introduces problems like the “diamond problem” and should be avoided

4. When a contract inherits from multiple contracts, only a single contract is created on the blockchain, and the code from all the base contracts is copied into the created contract

5. All function calls are virtual, which means that the most derived function is called, except when the contract name is explicitly given

6. When deploying a derived contract the base contract’s constructor is automatically called

7. `is` keyword is used when declaring a new derived contract

<br/>

## Abstract Contracts

<br/>

1. An `abstract` contract is the one with at least one function that is not implemented and is declared using the `abstract` keywork

2. You can mark a contract as being abstract even though all functions are implemented

3. **An abstract contract cannot be deployed**

<br/>

## Interfaces

<br/>

1. **Interfaces** are similar to **abstract** contracts, but they **cannot have any functions implemented**

2. Interfaces can be inherited

3. Interfaces have further restrictions

    * They cannot inherit from other contracts, but they can inherit from other interfaces
    * All declared functions must be external
    * They cannot declare a constructor
    * They cannot declare a constructor
     <br/>
     
4. An **interface** is created using the `interface` keyword instead of contract; 




<br/>

 ```
 //SPDX-License-Identifier: GPL-3.0

pragma solidity >= 0.5.0 < 0.9.0 ;

contract BaseContrcat{
    int public x;
    address public owner;

    constructor(){
        x = 5 ;
        owner = msg.sender ;
    }

    function setX(int _x) public{
        x = _x;
    }
}

contract A is BaseContract{
    int public y = 3;
}
 ```
<br/>

Let us consider the instance of contract **A** and notice that it inherits the code of it's base contract like it was copied into contract **A**. The constructor of the base contrcat is implicitly called when contrcat **A** is deployed.

<br/>


