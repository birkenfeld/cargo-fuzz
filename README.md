# Cargo-fuzz

Commandline wrapper for using libFuzzer. Easy to use, no need to recompile LLVM!


libFuzzer needs LLVM sanitizer support, so this is Linux-only for now. It also
may not work well with projects that have build scripts due to
https://github.com/rust-lang/cargo/issues/3739


## Installation

```sh
$ cargo install cargo-fuzz
```

## Usage

First, set up your project for fuzzing:

```sh
$ cd /path/to/project
$ cargo fuzz --init
```

This will create a `fuzz` folder, containing a fuzzing script called `fuzzer_script_1` in the
`fuzzers/` subfolder. It is generally a good idea to check in the files generated by `--init`.

libFuzzer is going to repeatedly call the `go()` function in the fuzzer script with a byte buffer
`data` of length `size`, until your program hits an error condition (segfault, panic, etc). Write
your `go()` function to hit the entry point you need.

You can add more fuzz target scripts via `cargo fuzz --add name_of_script`. There
is a `Cargo.toml` in the `fuzz/` folder where you can add dependencies.


To fuzz a fuzz target, run:

```sh
$ cd /path/to/project
$ cargo fuzz --fuzz-target fuzzer_script_1 # or whatever the target is named
```

Then, wait till it finds something!
