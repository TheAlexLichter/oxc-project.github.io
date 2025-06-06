---
title: Resolver
outline: deep
badges:
  - src: https://img.shields.io/crates/d/oxc_resolver
    alt: total downloads from crates.io
---

# Resolver

<AppBadgeList />

Node.js cjs and esm path resolution.

## Features

- All configurations are aligned with [webpack/enhanced-resolve][url-enhanced-resolve].
- 28x faster than [webpack/enhanced-resolve][url-enhanced-resolve] ([benchmark](https://github.com/oxc-project/bench-resolver)).
- See [README](https://github.com/oxc-project/oxc-resolver)

## Installation

### Node.js

- Use the node binding [oxc-resolver][url-oxc-resolver-npm].
- Try on [stackblitz](https://stackblitz.com/edit/oxc-resolver).

### Rust

See [https://crates.io/crates/oxc_resolver][url-oxc-resolver-crate] and its documentation [https://docs.rs/oxc_resolver][url-oxc-resolver-docs].

<!-- Links -->

[url-oxc-resolver-crate]: https://crates.io/crates/oxc_resolver
[url-oxc-resolver-docs]: https://docs.rs/oxc_resolver
[url-oxc-resolver-npm]: https://www.npmjs.com/package/oxc-resolver
[url-enhanced-resolve]: https://github.com/webpack/enhanced-resolve
