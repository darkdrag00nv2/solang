# Changelog
All notable changes to [Solang](https://github.com/hyperledger/solang/)
will be documented here.

## v0.2.2 Alexandria

### Added
- Solidity mappings can now have named key and named value types. [seanyoung](https://github.com/seanyoung)

### Changed
- Solang now uses LLVM 15. [LucasSte](https://github.com/LucasSte)
- Solidity on Solana now required the Anchor framework for the client code, and the `@solana/solidity.js`
  Typescript library is no longer compatible with Solidity.
- When casting hex literal numbers into the `bytesN` type, the hex literal may use leading zeros to match the size
with the according `bytesN`, which aligns solang with `solc`. [xermicus](https://github.com/xermicus)

### Fixed
- Many bugs have been fixed by [seanyoung](https://github.com/seanyoung), [LucasSte](https://github.com/LucasSte)
  and [xermicus](https://github.com/xermicus)
- Typos throughout the code have been fixed. [omahs](https://github.com/omahs)

## v0.2.1 Rio

### Added
- The Anchor IDL data structure is now generated for every Solana contract, although the actual IDL json file is not yet saved.
[LucasSte](https://github.com/LucasSte)

### Changed
- The Solana target now utilizes eight byte Anchor discriminators for function dispatch instead
of the four byte Ethereum selectors. [LucasSte](https://github.com/LucasSte)
- The deployment of contracts on Solana now follows the same scheme as Anchor. [seanyoung](https://github.com/seanyoung)
- Compares between rational literals and integers are not allowed. [seanyoung](https://github.com/seanyoung)
- Overriding the function selector value is now done using the `@selector([1, 2, 3, 4])`
  syntax, and the old syntax `selector=hex"12345678"` has been removed.
- `msg.sender` was not implemented correctly on Solana, and
  [has now been removed](https://solang.readthedocs.io/en/latest/targets/solana.html#msg-sender-solana).
  [seanyoung](https://github.com/seanyoung)
- Solang now uses LLVM 14. [LucasSte](https://github.com/LucasSte)

### Fixed
- Many bugs have been fixed by [seanyoung](https://github.com/seanyoung), [LucasSte](https://github.com/LucasSte)
  and [xermicus](https://github.com/xermicus)

## v0.2.0 Berlin
We are happy to release solang `v0.2.0` codenamed `Berlin` today. Aside from
containing many small fixes and improvements, this release marks a milestone
towards maturing our Substrate compilation target: any regressions building up
since `ink!` v3.0 are fixed, most notably the metadata format (shoutout and many
thanks to external contributor [extraymond](https://github.com/extraymond)) and
event topics. Furthermore, we are leaving `ink!` version 3 behind us, in favor
of introducing compatibility with the recent `ink!` 4 beta release and the latest
substrate contracts node `v0.22.1`.

### Added
- **Solana / breaking:** The try-catch construct is no longer permitted on Solana, as it
  never worked. Any CPI error will abort the transaction.
  [seanyoung](https://github.com/seanyoung)
- **Solana:** Introduce new sub-command `solang idl` which can be used for generating
  a Solidity interface file from an Anchor IDL file. This can be used for calling
  Anchor Contracts on Solana. [seanyoung](https://github.com/seanyoung)
- **Substrate:** Provide specific Substrate builtins via a "substrate" file. The
  `Hash` type from `ink!` is the first `ink!` specific type made available for Solidity
  contracts.
  [xermicus](https://github.com/xermicus)
- **Substrate:** Introduce the `--log-api-return-codes` CLI flag, which changes the
  emitted code to print return codes for `seal` API calls into the debug buffer.
  [xermicus](https://github.com/xermicus)
- Introduce function name mangling for overloaded functions and constructors, so
  that they can be represented properly in the metadata.
  [xermicus](https://github.com/xermicus)

### Changed
- The Solana target now uses Borsh encoding rather than eth abi
  encoding. This is aimed at making Solang contracts Anchor compatible.
  [LucasSte](https://github.com/LucasSte)
- **Substrate / breaking:** Supported node version is now pallet contracts `v0.22.1`.
  [xermicus](https://github.com/xermicus)
- **Substrate / breaking:** Remove the deprecated `random` builtin.
  [xermicus](https://github.com/xermicus)

### Fixed
- Whenever possible, the parser does not give up after the first error.
  [salaheldinsoliman](https://github.com/salaheldinsoliman)
- Constant expressions are checked for overflow.
  [salaheldinsoliman](https://github.com/salaheldinsoliman)
- AddMod and MulMod were broken. This is now fixed.
  [LucasSte](https://github.com/LucasSte)
- **Substrate / breaking:** Solang is now compatible with `ink!` version 4 (beta).
  [xermicus](https://github.com/xermicus)
- **Substrate:** Switched ABI generation to use official `ink!` crates, which fixes all
  remaining metadata regressions.
  [extraymond](https://github.com/extraymond) and [xermicus](https://github.com/xermicus)
- **Substrate:** Allow constructors to have a name, so that multiple constructors are
  supported, like in `ink!`.
  [xermicus](https://github.com/xermicus)
- All provided examples as well as most of the Solidity code snippets in our
  documentation are now checked for succesful compilation on the Solang CI.
  [xermicus](https://github.com/xermicus)
- **Substrate:** Fix events with topics. The topic hashes generated by Solang
  contracts are now exactly the same as those generated by `ink!`.
  [xermicus](https://github.com/xermicus)

## v0.1.13 Genoa

### Changed
- Introduce sub-commands to the CLI. Now we have dedicated sub-commands for
  `compile`, `doc`, `shell-completion` and the `language-server`, which makes
  for a cleaner CLI.
  [seanyoung](https://github.com/seanyoung)
- On Solana, emitted events are encoded with Borsh encoding following the Anchor
  format.
  [LucasSte](https://github.com/LucasSte)
- The ewasm target has been removed, since ewasm is not going to implemented on
  Ethereum. The target has been reused for an new EVM target, which is not complete
  yet.
  [seanyoung](https://github.com/seanyoung)
- Substrate: Concrete contracts must now have at least one public function. A
  public function is in a contract, if it has public or external functions, if
  it has a receive or any fallback function or if it has public storage items
  (those will yield public getters). This aligns solang up with `ink!`.
  [xermicus](https://github.com/xermicus)

### Added
- Solana v1.11 is now supported.
  [seanyoung](https://github.com/seanyoung)
- On Solana, programs now use a custom heap implementation, just like on
  Substrate. As result, it is now possible to `.push()` and `.pop()` on
  dynamic arrays in memory.
  [seanyoung](https://github.com/seanyoung)
- Arithmetic overflow tests are implemented for all integer widths,
  [salaheldinsoliman](https://github.com/salaheldinsoliman)
- Add an NFT example for Solana
  [LucasSte](https://github.com/LucasSte)
- Add a wrapper for the Solana System Program
  [LucasSte](https://github.com/LucasSte)
- The selector for functions can be overriden with the `selector=hex"abcd0123"`
  syntax.
  [seanyoung](https://github.com/seanyoung)
- Shell completion is available using the `solang shell-completion` subcommand.
  [xermicus](https://github.com/xermicus)
- Add support for the `create_program_address()` and `try_find_program_address()`
  system call on Solana
  [seanyoung](https://github.com/seanyoung)
- Substrate: The `print()` builtin is now supported and will write to the debug
  buffer. Additionally, error messages from the `require` statements will now be
  written to the debug buffer as well. The Substrate contracts pallet prints the
  contents of the debug buffer to the console for RPC ("dry-run") calls in case
  the `runtime::contracts=debug` log level is configured.
  [xermicus](https://github.com/xermicus)

### Fixed
- DocComments `/** ... */` are now permitted anywhere.
  [seanyoung](https://github.com/seanyoung)
- Function calls to contract functions via contract name are no longer possible,
  except for functions of base contracts.
  [xermicus](https://github.com/xermicus)

## v0.1.12 Cairo

### Added
- Added spl-token integration for Solana
- Solang now generates code for inline assembly, including many Yul builtins

### Changed
- The documentation has been re-arranged for readability.
- The solang parser can parse the same syntax as Ethereum Solidity 0.8.

### Fixed
- Fixed many parser issues. Now solang-parser parses all files in the
  Ethereum Solidity test suite. First run
  `git submodule update --init --recursive` to fetch the test files, and
  then run the test with `cargo test --workspace`.

## v0.1.11 Nuremberg

### Added
- Added support for Solidity user types
- Support `using` syntax on file scope
- Support binding functions with `using`
- Implemented parsing and semantic analysis of yul (code generation is to
  follow)
- The language server uses the `--import` and `--importmap` arguments
- On Solana, it is possible to set the accounts during CPI using the
  `accounts:` call argument.

### Fixed
- Fixed associativity of the power operator
- A huge amount of fixes improving compatibility with solc

## v0.1.10 Barcelona

### Added
- On Solana, the accounts that were passed into the transactions are listed in
  the `tx.accounts` builtin. There is also a builtin struct `AccountInfo`
- A new common subexpression elimination pass was added, thanks to
  [LucasSte](https://github.com/hyperledger/solang/pull/550)
- A graphviz dot file can be generated from the ast, using `--emit ast-dot`
- Many improvements to the solidity parser, and the parser has been spun out
  in it's own create `solang-parser`.

### Changed
- Solang now uses LLVM 13.0, based on the [Solana LLVM tree](https://github.com/solana-labs/llvm-project/)
- The ast datastructure has been simplified.
- Many bugfixes across the entire tree.

## v0.1.9

### Added
- Added support for solc import mapppings using `--importmap`
- Added support for Events on Solana
- `msg.data`, `msg.sig`, `msg.value`, `block.number`, and `block.slot` are
  implemented for Solana
- Implemented balance transfers using `.send()` and `.transfer()` on Solana
- Implemented retrieving account balances on Solana
- Verify ed25519 signatures with `signatureVerify()` on Solana
- Added support for Rational numbers
- The address type and value type can changed using `--address-length` and
  `--value-length` command line arguments (for Substrate only)

### Changed
- Solana now requires v1.8.1 or later
- On Solana, the return data is now provided in the program log. As a result,
  RPCs are now are now supported.
- On the solang command line, the target must be specified.
- The Solana instruction now includes a 64 bit value field
- Many fixes to the parser and resolver, so solidity compatibility is much
  improved, thanks to [sushi-shi](https://github.com/hyperledger/solang/pulls?q=is%3Apr+author%3Asushi-shi+is%3Aclosed).

### Removed
- The Sawtooth Sabre target has been removed.
- The generic target has been removed.

## v0.1.8

### Added
- Added a strength reduce pass to eliminate 256/128 bit multiply, division,
  and modulo where possible.
- Visual Studio Code extension can download the Solang binary from github
  releases, so the user is not required to download it themselves
- The Solana target now has support for arrays and mapping in contract
  storage
- The Solana target has support for the keccak256(), ripemd160(), and
  sha256() builtin hash functions.
- The Solana target has support for the builtins this and block.timestamp.
- Implement abi.encodePacked() for the ethereum abi encoder
- The Solana target now compiles all contracts to a single `bundle.so` BPF
  program.
- Any unused variables, events, or contract variables are now detected and
  warnings are given, thanks to [LucasSte](https://github.com/hyperledger/solang/pull/429)
- The `immutable` attribute on contract storage variables is now supported.
- The `override` attribute on public contract storage variables is now supported.
- The `unchecked {}` code block is now parsed and supported. Math overflow still
  is unsupported for types larger than 64 bit.
- `assembly {}` blocks are now parsed and give a friendly error message.
- Any variable use before it is given a value is now detected and results in
  a undefined variable diagnostic, thanks to [LucasSte](https://github.com/hyperledger/solang/pull/468)

### Changed
- Solang now uses LLVM 12.0, based on the [Solana LLVM tree](https://github.com/solana-labs/llvm-project/)

### Fixed
- Fix a number of issues with parsing the uniswap v2 contracts
- ewasm: staticcall() and delegatecall() cannot take value argument
- Fixed array support in the ethereum abi encoder and decoder
- Fixed issues in arithmetic on non-power-of-2 types (e.g. uint112)

## v0.1.7

### Added
- Added a constant folding optimization pass to improve codegen. When variables fold
  to constant values, they are visible in the hover in the extension
- For Substrate and Solana, address literals can specified with their base58 notation, e.g.
  `address foo = address"5GBWmgdFAMqm8ZgAHGobqDqX6tjLxJhv53ygjNtaaAn3sjeZ";`
- Solana account storage implemented for ``bytes``, ``string``, and structs
- Implemented ``delete`` for Solana

### Changed
- The Substrate target produces a single .contract file
- The Substrate target now uses the salt argument for seal\_instantiate()

### Fixed
- Libraries are allowed to have constant variables
- Fixed ethereum abi encoding/decoding of structs and enums
- Solana now returns an error if account data is not large enough
- Fixed storage bytes push() and pop()
- Ewasm uses precompiles for keccak hashing
- Various ewasm fixes for Hyperledger Burrow

## v0.1.6

### Added
- New Visual Studio Code extension developed under Hyperledger Mentorship
  programme
- Added language server for use in vscode extension
- Implemented primitives types and operations for Solana
- Functions can be declared outside of contracts
- Constants can be declared outside of contracts
- String formatting using python style "..{}..".format(n)

## v0.1.5

### Added
- Function types are implemented
- An experimental [Solana](https://solana.com/) target has been added
- Binaries are generated for Mac

### Changed
- The Substrate target requires Substrate 2.0

## v0.1.4

### Added
- `event` can be declared and emitted with `emit`
- Function modifiers have been implemented
- Tags in doc comments are parsed and resolved
- All major Solidity language features implemented, see our language status page:
  https://solang.readthedocs.io/en/latest/status.html

## v0.1.3

### Added
- `import` directives are supported
- New `--importpath` command line argument to specify directories to search for imports
- Contracts can have base contracts
- Contracts can be abstract
- Interfaces are supported
- Libraries are supported
- The `using` library `for` type syntax is supported

### Changed
- Solang now uses llvm 10.0 rather than llvm 8.0
- In line with Solidity 0.7.0, constructors no longer need a visibility argument
