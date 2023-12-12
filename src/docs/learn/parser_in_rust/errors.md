---
title: Dealing with Errors
outline: deep
---

# Dealing with Errors

Quoting from the [Dragon Book](https://www.amazon.com/Compilers-Principles-Techniques-Tools-2nd/dp/0321486811)

> Most programming language specifications do not describe how a compiler should respond to errors; error handling is left to the compiler designer.
> Planning the error handling right from the start can both simplify the structure of a compiler and improve its handling of errors.

A fully recoverable parser can construct an AST no matter what we throw at it.
For tools such as linter or formatter, one would wish for a fully recoverable parser so we can act on part of the program.

A panicking parser will abort if there is any grammar mismatch, and a partially recoverable parser will recover from deterministic grammars.

For example, given a grammatically incorrect while statement `while true {}`, we know it is missing round brackets,
and the only punctuation it can have are round brackets, so we can still return a valid AST and indicate its missing brackets.

Most JavaScript parsers out there are partially recoverable, so we'll do the same and build a partially recoverable parser.

:::info
The Biome parser is a fully recoverable parser.
:::

Rust has the `Result` type for returning and propagating errors.
In conjunction with the `?` syntax, the parse functions will remain simple and clean.

It is common to wrap the Result type so we can replace the error later:

```rust
pub type Result<T> = std::result::Result<T, ()>;
```

Our parse functions will return a Result, for example:

```rust
pub fn parse_binding_pattern(&mut self, ctx: Context) -> Result<BindingPattern<'a>> {
    match self.cur_kind() {
        Kind::LCurly => self.parse_object_binding_pattern(ctx),
        Kind::LBrack => self.parse_array_binding_pattern(ctx),
        kind if kind.is_binding_identifier() => {
          // ... code omitted
        }
        // highlight-next-line
        _ => Err(()),
    }
}
```

We can add an `expect` function for returning an error if the current token does not match the grammar:

```rust
/// Expect a `Kind` or return error
pub fn expect(&mut self, kind: Kind) -> Result<()> {
    if !self.at(kind) {
        return Err(())
    }
    self.advance();
    Ok(())
}
```

And use it as such:

```rust
pub fn parse_paren_expression(&mut self, ctx: Context) -> Result<Expression> {
    self.expect(Kind::LParen)?;
    let expression = self.parse_expression(ctx)?;
    self.expect(Kind::RParen)?;
    Ok(expression)
}
```

:::info

For completeness, the lexer function `read_next_token` should also return `Result`
when an unexpected `char` is found when lexing.

:::

### The `Error` Trait

To return specific errors, we need to fill in the `Err` part of `Result`:

```rust
pub type Result<T> = std::result::Result<T, SyntaxError>;
                                            ^^^^^^^^^^^
#[derive(Debug)]
pub enum SyntaxError {
    UnexpectedToken(String),
    AutoSemicolonInsertion(String),
    UnterminatedMultiLineComment(String),
}
```

We call it `SyntaxError` because all "early error"s defined in the grammar section of the ECMAScript specification are syntax errors.

To make this a proper `Error`, it needs to implement the [`Error` Trait](https://doc.rust-lang.org/std/error/trait.Error.html). For cleaner code, we can use macros from the [`thiserror`](https://docs.rs/thiserror/latest/thiserror) crate:

```rust
#[derive(Debug, Error)]
pub enum SyntaxError {
    #[error("Unexpected Token")]
    UnexpectedToken,

    #[error("Expected a semicolon or an implicit semicolon after a statement, but found none")]
    AutoSemicolonInsertion,

    #[error("Unterminated multi-line comment")]
    UnterminatedMultiLineComment,
}
```

We can then add an `expect` helper function for throwing an error if the token does not match:

```rust
/// Expect a `Kind` or return error
pub fn expect(&mut self, kind: Kind) -> Result<()> {
    if self.at(kind) {
        return Err(SyntaxError::UnExpectedToken);
    }
    self.advance(kind);
    Ok(())
}
```

The `parse_debugger_statement` can now use the `expect` function for proper error management:

```rust
fn parse_debugger_statement(&mut self) -> Result<Statement> {
    let node = self.start_node();
    self.expect(Kind::Debugger)?;
    Ok(Statement::DebuggerStatement {
        node: self.finish_node(node),
    })
}
```

Notice the `?` after the `expect`,
it is a syntactic sugar called the ["question mark operator"](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator) for making the
function return early if the `expect` function returns a `Err`.

### Fancy Error Report

[`miette`](https://docs.rs/miette/latest/miette) is one of the nicest error reporting crate out there,
it provides a fancy colored output

![miette](https://raw.githubusercontent.com/zkat/miette/main/images/serde_json.png)

Add `miette` to your `Cargo.toml`

```toml
[dependencies]
miette = { version = "5", features = ["fancy"] }
```

We can wrap our `Error` with `miette` and not modify the `Result` type defined in our parser:

```rust
pub fn main() -> Result<()> {
    let source_code = "".to_string();
    let file_path = "test.js".to_string();
    let mut parser = Parser::new(&source_code);
    parser.parse().map_err(|error| {
        miette::Error::new(error).with_source_code(miette::NamedSource::new(file_path, source_code))
    })
}
```
