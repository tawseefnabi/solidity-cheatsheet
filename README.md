# Solidity Cheatsheet and Best practices


## Table of contents
 * [SPDX License Identifier](#SPDX-License-Identifier)
 * [Pragma](#Pragma)
 * [Data Types](#Data-Types)
    + [Boolean](#Boolean)
    + [Integer](#Integer)
    + [Address](#Address)
    + [Array](#Array)
    + [Enum](#Enum)
    + [Struct](#Struct)
    + [Mapping](#Mapping)



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