# Solidity Cheatsheet and Best practices


## Table of contents
 * [SPDX License Identifier](#SPDX-License-Identifier)
 * [Pragma](#Pragma)
 *[Import](#Import)
 * [Data Types](#Data-Types)
    + [Boolean](#Boolean)
    + [Integer](#Integer)
    + [Address](#Address)
    + [Array](#Array)
    + [Enum](#Enum)
    + [Struct](#Struct)
    + [Mapping](#Mapping)
 * [Variables](#Variables)
    + [Local](#Local)
    + [State](#State)
    + [Global](#Global)
 * [Constants](#Constants)
 * [Functions](#Functions)
    + [Access Modifiers](#Access-Modifiers)
    + [Parameters](#Parameters)
      - [Input Paramters](#Input-Parameters)
      - [Output Parameters](#Output-Parameters)
      
 * [Control Structures](#Control-Structures)

## SPDX-License-Identifier
  the Solidity compiler encourages the use of machine-readable SPDX license identifiers.Every source file should start with a comment indicating its license:

`// SPDX-License-Identifier: MIT`

If you do not want to specify a license or if the source code is not open-source, please use the special value `UNLICENSED`.
The comment is recognized by the compiler anywhere in the file at the file level, but it is recommended to put it at the top of the file.
More information about how to use SPDX license identifiers can be found at the [SPDX website](https://spdx.dev/ids/#how).
## Pragma

 The `pragma` keyword is used to enable certain compiler features or checks. 
 The version pragma is used as follows: `pragma solidity >=0.4.22 <0.9.0;`
  A source file with the line above does not compile with a compiler earlier than version 0.4.22, and it also does not work on a compiler starting from version 0.9.0 (). Because there will be no breaking changes until version 0.9.0, you can be sure that your code compiles the way you intended. The exact version of the compiler is not fixed, so that bugfix releases are still possible.

## Import
  You can import local and external files in Solidity.
  ### **Local**
  The import directive is used to import a local file.

  ```sh
  ├── Import.sol
└── Foo.sol
  ```
  Fool.sol
  ```sh
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.10;

contract Foo {
    string public name = "Foo";
}
  ```
import.sol
```sh
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.10;
  // import Foo.sol from current directory
import "./Foo.sol";

contract Import {
    // Initialize Foo.sol
    Foo public foo = new Foo();

    // Test Foo.sol by getting it's name.
    function getFooName() public view returns (string memory) {
        return foo.name();
    }
}
```

### **External**
You can also import from **GitHub** by simply copying the url
```sh
// https://github.com/owner/repo/blob/branch/path/to/Contract.sol
import "https://github.com/owner/repo/blob/branch/path/to/Contract.sol";

// Example import ECDSA.sol from openzeppelin-contract repo, release-v3.3 branch
// https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.3/contracts/cryptography/ECDSA.sol
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.3/contracts/cryptography/ECDSA.sol";

```
```sh
import "filename";

import * as symbolName from "filename"; or import "filename" as symbolName;

import {symbol1 as alias, symbol2} from "filename";
```
## Data Types
### Boolean
  
  `bool`: The possible values are constants `true` and `false`.

`bool public boo = true;`

### Integer
`int` / `uint`: Signed and unsigned integers of various sizes. Keywords `uint8` to `uint256` in steps of 8 (unsigned of 8 up to 256 bits) and `int8` to `int256`. 
 
 **Default**: 
 
`uint` and `int` are aliases for `uint256` and `int256`, respectively.

**Unsigned** : uint8 | uint16 | uint32 | uint64 | uint128 | uint256(uint)

**Signed** : int8 | int16 | int32 | int64 | int128 | int256(int)

uint stands for unsigned integer, meaning non negative integers, different sizes are available
  - uint8   ranges from 0 to 2 ** 8 - 1
  - uint16  ranges from 0 to 2 ** 16 - 1
  - uint32  ranges from 0 to 2 ** 32 - 1
  - uint64  ranges from 0 to 2 ** 64 - 1
  - uint128  ranges from 0 to 2 ** 128 - 1
  - uint256 ranges from 0 to 2 ** 256 - 1

   **example**

  uint8 public u8 = 1;

  uint public u256 = 456;

  uint public u = 123; // uint is an alias for uint256

Negative numbers are allowed for int types. Like uint, different ranges are available from int8 to int256
    
  - int256 ranges from -2 ** 255 to 2 ** 255 - 1
  - int128 ranges from -2 ** 127 to 2 ** 127 - 1 

  **example**:

  int8 public i8 = -1;

  int public i256 = 456;

  int public i = -123;     // int is same as int256

  **minimum and maximum of int**

    int public minInt = type(int).min;
    int public maxInt = type(int).max;

  **Fixed Point Numbers**

  Fixed point numbers are not fully supported by Solidity yet. They can be declared, but cannot be assigned to or from.
  `fixed` / `ufixed`: Signed and unsigned fixed point number of various sizes.

  ### Address

  The address type comes in two flavours, which are largely identical:
   - `address`: Holds a **20** byte value (size of an Ethereum address).

  ```sh
    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;
  ```

   - `address` payable: Same as address, but with the additional members **transfer** and **send**.

  #### **Methods**

  **balance**
  
  `<address>.balance (uint256)`: balance of the Address in Wei

  **Transfer and Send**
  - `<address>.transfer(uint256 amount)`: send given amount of Wei to Address, throws on failure
  - `<address>.send(uint256 amount) returns (bool)`: send given amount of Wei to Address, returns false on failure

   The idea behind this distinction is that `address payable` is an address you can send Ether to, while a plain address cannot be sent Ether.
  ### Array

  Arrays can be dynamic or have a fixed size.

  `uint[7]`: fixed-sized byte arrays.

  `uint[]`: dynamically-sized byte arrays.

  ### Enum

  Solidity supports enumerables and they are useful to model choice and keep track of state.
  Enums can be declared outside of a contract.

  **Enum representing shipping status**

  ```sh 
  enum Status {
        Pending,
        Shipped,
        Accepted,
        Rejected,
        Canceled
  }
```
  ### Struct

You can define your own type by creating a `struct`.

They are useful for grouping together related data.

Structs can be declared outside of a contract and imported in another contract.

```sh
struct Todo {
        string text;
        bool completed;
    }
Todo todos;
```
### Mapping

Mapping in Solidity acts like a hash table or dictionary in any other language
Maps are created with the syntax 

`mapping(key => value) <access specifier> <name>;`

`key` can be value types such as `uint, address or bytes` except for a `mapping`, a `dynamically sized array`, a `contract`, an `enum`, or a `struct`.

`value` can be any type including another mapping or an array.

 // Mapping from address to uint

`mapping(address => uint) public myMap;`

### Variables

There are 3 types of variables in Solidity
 - **Local**
    + declared inside a function
    + not stored on the blockchain 
 - **State**
    + declared outside a function
    + stored on the blockchain

 - **Global**(provides information about the blockchain)

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Variables {
    // State variables are stored on the blockchain.
    string public text = "Hello";
    uint public num = 123;

    function doSomething() public {
        // Local variables are not saved to the blockchain.
        uint i = 456;

        // Here are some global variables
        uint timestamp = block.timestamp; // Current block timestamp
        address sender = msg.sender; // address of the caller
    }
}
```
### Constants

Constants are variables that cannot be modified.

Their value is hard coded and using constants can save gas cost.

***coding convention to uppercase constant variables***

```sh
address public constant MY_ADDRESS = 0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc;
uint public constant MY_UINT = 123;
```

### Functions
There are several ways to return outputs from a function.

Public functions cannot accept certain data types as inputs or outputs
```sh
function function_name(<parameter types>) {internal|external|public|private} [pure|constant|view|payable] [returns (<return types>)]
```
### Access-Modifiers

- `public` - Accessible from this contract, inherited contracts and externally
- `private` - Accessible only from this contract
- `internal` - Accessible only from this contract and contracts inheriting from it
- `external` - Cannot be accessed internally, only externally. Recommended to reduce gas. Access internally with this.f

The visibility specifier is given after the type for state variables and between parameter list and return parameter list for functions.
```sh
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract C {
    uint private data;

    function f(uint a) private pure returns(uint b) { return a + 1; }
    function setData(uint a) public { data = a; }
    function getData() public view returns(uint) { return data; }
    function compute(uint a, uint b) internal pure returns (uint) { return a + b; }
}

// This will not compile
contract D {
    function readData() public {
        C c = new C();
        uint local = c.f(7); // error: member `f` is not visible
        c.setData(3);
        local = c.getData();
        local = c.compute(3, 5); // error: member `compute` is not visible
    }
}

contract E is C {
    function g() public {
        C c = new C();
        uint val = compute(3, 5); // access to internal member (from derived to parent contract)
    }
}
```

### Parameters
### Input-Parameters

Parameters are declared just like variables and are memory variables.

`function f(uint _a, uint _b) {}`

### Output-Parameters

Output parameters are declared after the returns keyword.

`function f(uint _a, uint _b) returns (uint _sum) {
   _sum = _a + _b;
}`


Output can also be specified using return statement. In that case, we can omit parameter name returns (uint).

Multiple return types are possible with return (v0, v1, ..., vn).

### Constructors
### View, Constant and Pure Functions
### Payable Functions

### Modifiers 

Modifiers can be used to change the behaviour of functions in a declarative way. For example, you can use a modifier to automatically check a condition prior to executing the function.
they can be used for:
 - Restrict  access
 - Validate inputs
 - Guard against reentrancy hack

 ```sh

  // Modifier to check that the caller is the owner of
  // the contract

 modifier onlyOwner {
    require(msg.sender == owner);
      // Underscore is a special character only used inside
      // a function modifier and it tells Solidity to
      // execute the rest of the code.
    _;
  }

  // Modifiers can take inputs. This modifier checks that the
  // address passed in is not the zero address.

  modifier validAddress(address _addr) {
        require(_addr != address(0), "Not valid address");
        _;
    }

  function changeOwner(address _newOwner) public onlyOwner validAddress(_newOwner) {
        owner = _newOwner;
    }

  // Modifiers can be called before and / or after a function.
  // This modifier prevents a function from being called while
  // it is still executing.
  modifier noReentrancy() {
    require(!locked, "No reentrancy");
      locked = true;
      _;
      locked = false;
  }

  function decrement(uint i) public noReentrancy {
    x -= i;
    if (i > 1) {
      decrement(i - 1);
    }
  }
 ```
### Control-Structures
  Most of the control structures known from curly-braces languages are available in Solidity except for switch and goto.

+ if else
+ while
+ do
+ for
+ break
+ continue
+ return
+ ? :

**Try-Catch**

 `try / catch` can only catch errors from external function calls and contract creation.

### Inheritance

Solidity supports multiple inheritance. Contracts can inherit other contract by using the is keyword.

Function that is going to be overridden by a child contract must be declared as `virtual`.

Function that is going to override a parent function must use the keyword `override`.

Order of inheritance is important.

You have to list the parent contracts in the order from “most base-like” to “most derived”.

```sh

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

/* Graph of inheritance
    A
   / \
  B   C
 / \ /
F  D,E

*/

contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// Contracts inherit other contracts by using the keyword 'is'.
contract B is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}

// Contracts can inherit from multiple parent contracts.
// When a function is called that is defined multiple times in
// different contracts, parent contracts are searched from
// right to left, and in depth-first manner.

contract D is B, C {
    // D.foo() returns "C"
    // since C is the right most parent contract with function foo()
    function foo() public pure override(B, C) returns (string memory) {
        return super.foo();
    }
}

contract E is C, B {
    // E.foo() returns "B"
    // since B is the right most parent contract with function foo()
    function foo() public pure override(C, B) returns (string memory) {
        return super.foo();
    }
}

// Inheritance must be ordered from “most base-like” to “most derived”.
// Swapping the order of A and B will throw a compilation error.
contract F is A, B {
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo();
    }
}
```

### Virtual
### Override