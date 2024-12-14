---
title: Parser
outline: deep
---

# Parser

<AppBadgeList />

This is production ready.

## Features

- 3x faster than swc parser ([benchmark][url-benchmark]).
- Parses `.js(x)` and `.ts(x)`.
- Passes all parser tests from Test262 and 99% from Babel and TypeScript.
- Returns ESM information directly, no need for [`es-module-lexer`](https://github.com/guybedford/es-module-lexer).
- [✅ works with checker.ts](https://x.com/robpalmer2/status/1805502964435505559)

## Installation

### Rust

Use the umbrella crate [oxc][url-oxc-crate] or the individual [oxc_ast][url-oxc-ast-crate] and [oxc_parser][url-oxc-parser-crate] crates.

Rust usage example can be found [here](https://github.com/oxc-project/oxc/blob/main/crates/oxc_parser/examples/parser.rs).

### Node.js (NAPI)

Try the experimental node binding [oxc-parser][url-oxc-parser-npm].

<!-- Links -->

[url-swc]: https://swc.rs
[url-benchmark]: https://github.com/oxc-project/bench-javascript-parser-written-in-rust
[url-oxc-crate]: https://docs.rs/oxc
[url-oxc-ast-crate]: https://docs.rs/oxc_ast
[url-oxc-parser-crate]: https://docs.rs/oxc_parser
[url-oxc-parser-npm]: https://www.npmjs.com/package/oxc-parser
