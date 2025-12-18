---
title: Rust
author: David Lewis
---

Rust is a programming language meant to replace traditionally "difficult"
compiled languages like C and C++. Rust can be used for almost any application,
data science included.

## Getting started on the cluster

The traditional way to install Rust is via its package manager: `rustup`. Now,
rustup is the "toolchain manager", but you can think of it as a package manager,
but only for different versions of Rust. This should already be installed in the
cluster, you can simply type `rustup` to get started.

Now, `rustup` has lots of capabilities, there is even a
[book about it](https://rust-lang.github.io/rustup/concepts/toolchains.html),
but the only rustup command you should ever have to run is the following:

```bash
rustup default stable
```

This will install the stable version of Rust and set it as the default, which is
fine in most cases.

### Hello, world

Once, you've installed Rust, you can create a sample project. Make a directory
you would like to have your rust project in, then run the following:

```bash
mkdir myproject # create your project directory
cd myproject # go inside the directory
cargo init # sets up a rust project
```

Cargo is rust's package manager for packages, known as "crates". It also works
as a build system, like Make or Scons. By default, `cargo init` sets up the
project as a "hello, world" program. To compile and run the project, run the
following:

```bash
cargo run # this should print "hello, world"
```

The entry point of a Rust program is `main.rs`, so feel free to experiment in
there. Rust is designed to work with editors, so you're really missing out if
you're just using the plain `vim` or `nano`. Consider trying out [[vscode]] with
the Rust extension.

## Resources

One of the great things about Rust is its ecosystem. The Rust organization
provides a tool called [mdbook](https://github.com/rust-lang/mdBook), which
makes it easy to create static website "books" in and outside the rust
ecosystem.

Rust (the organization) provides two books:

- ["The Book"](https://doc.rust-lang.org/book/): great if you want theory or
  extended explanations about Rust code
- ["Rust by Example"](https://doc.rust-lang.org/rust-by-example/): Great for
  quick and dirty learning, and a good no-nonsense reference when you just need
  it to work

Additionally, Rust provides an interactive course meant to be run locally:

- ["Rustlings"](https://rustlings.rust-lang.org/): never tried it, but looks
  really great!

### Videos

Here is a playlist of videos that may sell you on rust. They are short and easy
to digest.

- [Rust Talks](https://www.youtube.com/watch?v=Q3AhzHq8ogs&list=PLZaoyhMXgBzoM9bfb5pyUOT3zjnaDdSEP):
  No Boilerplate

## Alternative ways of running rust

Rust is traditionally a compiled language, though the editor support sidesteps
many of the problems associated with compiled languages (runtime uncertainty).
That being said, there are ways of interpreting Rust, rather than compiling it.

The central project creating interpreter support for Rust is
[evcxr](https://github.com/evcxr/evcxr), which provides an interactive repl as
well as a jupyter kernel for use with jupyter notebooks.
