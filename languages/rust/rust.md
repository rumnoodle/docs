# Rust Programming Language

## rustup

$ `rustup update` updates rust.

$ `rustup self uninstall` uninstalls rust.

$ `rustup doc` opens local documentation in browser.

## cargo

A lot of external libraries and tools can be found at crates.io, just add them as dependencies to Cargo.toml and use them in your projects.

$ `cargo new <project name>` creates a project with the name project name. With the `--vcs` option it is possible to set which, if any, version control software to use.

$ `cargo build` builds a cargo project. `--release` flag makes optimizations for release builds.

$ `cargo check` checks that it compiles but doesn't build it. Good when developing as it runs faster than build.

$ `cargo run` runs a cargo project and builds it if it hasn't been built.

$ `cargo update` updates the dependencies in Cargo.toml.

$ `cargo doc --open` opens documentation of project and dependencies in browser.

## Compiling

$ `rustc <filename.rs>` compiles a rust file.

## Formatting

$ `rustfmt <filename.rs>` formats a rust file to follow code conventions.

## Syntax

### Comments

`// This is a comment`

### Imports

```
use std::io; // a Rust library dependency
use rand::Rng; // an external dependency added in Cargo.toml

### Variables

```
let variable_name = "value"; // creates an immutable variable
let mut variable_name = "value"; // creates a mutable variable
```

### Strings

#### println!()

Placeholders in strings: `"Replace {this} with the contents of variable named this."`.
