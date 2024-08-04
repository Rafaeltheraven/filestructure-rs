# `filestructure-rs`

A library to sloppily add data to a filestructure in memory and write to disk whenever you want.

## How to Use
The inner `FileStructure` is essentially a linked-list of a classic unix-style filetree. First, we can construct our main empty structure either using the `FileStructure::default` implementation (which will give you an empty directory without a name) or by using `FileStructure::from_path` which takes a unix-style path and creates a structure that mimics it.

```rust
let mut fs = FileStructure::from_path("/some/path") // This creates a filestructure with a root node, a subdir of the root called 'some' and a subdir of 'some' called 'path'.
let mut new = fs.insert_dir("/some/other") // We now add a new subdirectory of 'some' called 'other' and return a handle to it.
new.insert_string("hello.txt", String::from("world")) // Create a new file /some/other/hello.txt with the string 'world' in it.
new.insert_blob("data.obj", Box::new([0,0,0,0])) // Create a new binary file /some/other/data.obj with some byte data.
fs.write_to_disk("./") // Write this file structure to disk starting from our CWD.
```

Additionally, if you enable to `tokenstream2` feature flag, you can add `proc_macro2::TokenStream` to the structure using `insert_tokenstream`. See the documentation for how that works.

If you ever lose track of a handle to some point in the filesystem, you can use `fs.get("/handle/to/find")` in order to search the structure and get a reference to a matching node (or `get_mut` for a mutable reference).