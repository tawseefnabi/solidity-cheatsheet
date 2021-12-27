# Solidity Cheatsheet and Best practices


## Table of contents
 * [SPDX-License-Identifier](#SPDX-License-Identifier)
 * [Pragma](#pragma)



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
