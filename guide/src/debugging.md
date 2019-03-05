# Debugging

## Macros

Pyo3's attributes, `#[pyclass]`, `#[pymodule]`, etc. are [procedural macros](https://doc.rust-lang.org/unstable-book/language-features/proc-macro.html), which means that rewrite the source of the annotated item. You can view the generated source with the following command, which also expands a few other things:

```bash
cargo rustc --profile=check -- -Z unstable-options --pretty=expanded > expanded.rs; rustfmt expanded.rs
```

(You might need to install [rustfmt](https://github.com/rust-lang-nursery/rustfmt) if you don't already have it.)

You can also debug classic `!`-macros by adding -Z trace-macros`:

```bash
cargo rustc --profile=check -- -Z unstable-options --pretty=expanded -Z trace-macros > expanded.rs; rustfmt expanded.rs
```

See [cargo expand](https://github.com/dtolnay/cargo-expand) for a more elaborate version of those commands.

## Running with Valgrind

Valgrind is a tool to detect memory managment bugs such as memory leaks.

You first need to installa debug build of python, otherwise valgrind won't produce usable results. In ubuntu there's e.g. a `python3-dbg` package.

Activate an environment with the debug interpreter and recompile. If you're on linux, use `ldd` with the name of you're binary and check that you're linking e.g. `libpython3.6dm.so.1.0` instead of `libpython3.6m.so.1.0`.

[Download the suppressions file for cpython](https://raw.githubusercontent.com/python/cpython/master/Misc/valgrind-python.supp).

Run valgrind with `valgrind --suppressions=valgrind-python.supp ./my-command --with-options`
