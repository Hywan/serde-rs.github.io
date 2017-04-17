# Setting up codegen

Serde provides code generation to generate implementations of the `Serialize`
and `Deserialize` traits for data structures defined in your crate, allowing
them to be used conveniently with all of Serde's data formats.

**You only need codegen if your code is using `#[derive(Serialize,
Deserialize)]`.**

Codegen is based on Rust's `#[derive]` mechanism, just like what you would use
to automatically derive implementations of the built-in `Clone`, `Copy`, or
`Debug` traits. It is able to generate implementations for most structs and
enums including ones with elaborate generic types or trait bounds. On rare
occasions, for an especially convoluted type you may need to [implement the
traits manually](custom-serialization.md).

These derives require a Rust compiler version 1.15 or newer.

Here is the `Cargo.toml`:

!FILENAME Cargo.toml
```toml
[package]
name = "my-crate"
version = "0.1.0"
authors = ["Me <user@rust-lang.org>"]

[dependencies]
serde = "0.9"
serde_derive = "0.9"

# serde_json is just for the example, not required in general
serde_json = "0.9"
```

Now the `src/main.rs` which uses Serde's custom derives:

!FILENAME src/main.rs
!PLAYGROUND 454091616f81d99a48e72d1c5a430f2a
```rust
#[macro_use]
extern crate serde_derive;

extern crate serde;
extern crate serde_json;

#[derive(Serialize, Deserialize, Debug)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 1, y: 2 };

    let serialized = serde_json::to_string(&point).unwrap();
    println!("serialized = {}", serialized);

    let deserialized: Point = serde_json::from_str(&serialized).unwrap();
    println!("deserialized = {:?}", deserialized);
}
```

Here is the output:

```
$ cargo run
serialized = {"x":1,"y":2}
deserialized = Point { x: 1, y: 2 }
```