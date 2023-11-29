# A Brief introduction to Rust

_This repository was made for an FCC Algarve meeting on the 29th of November_

Rust is a strongly typed language with roots on strong memory safety and 
simplicity. Wait... How can a strongly typed memory safe language exist that
is simple enough for mere humans to understand? Well, here's what makes this
language interesting:

- Implicit Types
  - Unlike Rust's lower-level cousins, you don't have to define types for the
  compiler to be happy about your variables
- Owned Variables
  - Rust's variables can only be owned by one variable. This means that if you
  pass a variable to a function, you will no longer be able to access that
  variable
- Informative Compiler Errors
  - Some Unix socks wearing programmers spent thousands of hours making sure
  that the compile errors you get are as informative as possible. Even
  providing soutions sometimes
- No-nullability
  - No variable in Rust can be null, this forces the programmer to always
  handle errors
- Non-mutable by Default
  - Variables that are mutable need to be expressively typed. This lets the
  compiler know where to alocate the memory for these variables
- Deep Dev Enviroment
  - Rust's components rustup, cargo, clippy, rustfmt and crates provide the
  developer with all the tools necessary to create perfect code

Now that we have had a simple overview, let's install Rust.

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

This should install the most basic components of Rust mentioned previously.
Now let's go through some simple code. Create a new cargo project with:

```sh
# Cargo crates are always in kebab-case
cargo new fcc29rust
cd fcc29rust
```

```rust
// All rust programs start with a main function (mostly)
fn main() {
  // Here we assign a variable, strings in rust are special, more on that later
  let a = "World".to_string();
  // This is a macro call, more on that later
  println!("Hello {}", a);
}
```

Now you can run:

```sh
cargo run
```

This one command will download all the dependencies required, compile the code
and run the program after compilation. You should see:

```
$ cargo run
Hello World
```

Now let's try a little something different, let's move some variables around.

```rust
fn main() {
  let a = "World".to_string();
  let b = a;
  println!("Hello {} {}", a, b);
}
```

Now let's try to run this program with `cargo run`... Oh...

```
  error[E0382]: borrow of moved value: `a`
 --> src/main.rs:4:35
  |
2 |     let a = "World".to_string();
  |         - move occurs because `a` has type `String`, which does not implement the `Copy` trait
3 |     let b = a;
  |             - value moved here
4 |     println!("Hello world {} {}", a, b);
  |                                   ^ value borrowed here after move
  |
  = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
3 |     let b = a.clone();
  |              ++++++++

For more information about this error, try `rustc --explain E0382`.
error: could not compile `fcc29rust` (bin "fcc29rust") due to previous error
```

Here we discover Rust's memory safety feature.
