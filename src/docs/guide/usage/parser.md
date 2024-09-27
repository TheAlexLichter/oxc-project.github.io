---
title: Parser
outline: deep
badges:
  - src: https://img.shields.io/crates/d/oxc_parser
    alt: total downloads from crates.io
---

# Parser

<AppBadgeList />

The Rust crate `oxc_parser` is production ready.

## Features

- 3x faster than swc parser ([benchmark][url-benchmark]).
- Parses `.js(x)` and `.ts(x)`.
- Passes all parser tests from Test262 and 99% from Babel and TypeScript.
- Utility for extracting ESM import and export data - a better [`es-module-lexer`](https://github.com/guybedford/es-module-lexer).
- [✅ works with checker.ts](https://x.com/robpalmer2/status/1805502964435505559)

## Installation

### Rust

Use the umbrella crate [oxc][url-oxc-crate] or the individual [oxc_ast][url-oxc-ast-crate] and [oxc_parser][url-oxc-parser-crate] crates.

Rust usage example can be found [here](https://github.com/oxc-project/oxc/blob/main/crates/oxc_parser/examples/parser.rs).

### Node.js (NAPI)

Use the experimental node binding [oxc-parser][url-oxc-parser-npm].

### ESM Module Lexer

- Use the npm package `oxc-parser` with the API `moduleLexerSync`.
- Use the crate [oxc_module_lexer](https://crates.io/crates/oxc_module_lexer).

<!-- Links -->

[url-swc]: https://swc.rs
[url-benchmark]: https://github.com/oxc-project/bench-javascript-parser-written-in-rust
[url-oxc-crate]: https://docs.rs/oxc
[url-oxc-ast-crate]: https://docs.rs/oxc_ast
[url-oxc-parser-crate]: https://docs.rs/oxc_parser
[url-oxc-parser-npm]: https://www.npmjs.com/package/oxc-parser
