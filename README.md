# Format

[![crates.io](https://img.shields.io/crates/v/format.svg)](https://crates.io/crates/format)
[![docs.rs](https://docs.rs/format/badge.svg)](https://docs.rs/format)
[![license](https://img.shields.io/crates/l/format)](#license)
[![ci](https://github.com/kgv/format/workflows/ci/badge.svg)](https://github.com/kgv/format/actions)
[![minimum supported rust version](https://img.shields.io/badge/rust-1.45-yellow.svg)](https://github.com/kgv/format#minimum-supported-rust-version)

A utility crate to make it easier to work with the formatter

## Usage

Add dependency to your `Cargo.toml`:

```toml
[dependencies]
format = "0.2"
```

and use `lazy_format` macro:

```rust
struct Foo(usize);

impl Debug for Foo {
    fn fmt(&self, f: &mut Formatter) -> Result {
        let alternate = f.alternate();
        let bar = lazy_format!(|f| if alternate {
            write!(f, "{:#x}", self.0)
        } else {
            write!(f, "{:x}", self.0)
        });
        f.debug_tuple("Foo")
            .field(&format_args!("{}", bar))
            .finish()
    }
}
```

or one of format type:

```rust
struct Foo(usize);

impl Debug for Foo {
    fn fmt(&self, f: &mut Formatter) -> Result {
        let alternate = f.alternate();
        let bar = LowerHex(|f| {
            if alternate {
                write!(f, "{:#x}", self.0)
            } else {
                write!(f, "{:x}", self.0)
            }
        });
        f.debug_tuple("Foo")
            .field(&format_args!("{:x}", bar))
            .finish()
    }
}
```
