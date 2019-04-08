# pkg-config-rs

[![Build Status](https://travis-ci.com/rust-lang/pkg-config-rs.svg?branch=master)](https://travis-ci.com/rust-lang/pkg-config-rs)
[![Rust](https://img.shields.io/badge/rust-1.28%2B-blue.svg?maxAge=3600)](https://github.com/alexcrichton/pkg-config-rs/)

[Documentation](https://docs.rs/pkg-config)

A simple library meant to be used as a build dependency with Cargo packages in
order to use the system `pkg-config` tool (if available) to determine where a
library is located.

You can use this crate directly to probe for specific libraries, or use
[metadeps](https://github.com/joshtriplett/metadeps) to declare all your
`pkg-config` dependencies in `Cargo.toml`.

This library requires Rust 1.28+.

# Example

Find the system library named `foo`, with minimum version 1.2.3:

```rust
extern crate pkg_config;

fn main() {
    pkg_config::Config::new().atleast_version("1.2.3").probe("foo").unwrap();
}
```

Find the system library named `foo`, with no version requirement (not
recommended):

```rust
extern crate pkg_config;

fn main() {
    pkg_config::probe_library("foo").unwrap();
}
```

# External configuration via target-scoped environment variables

In cross-compilation context, it is useful to manage separately PKG_CONFIG_PATH
and a few other variables for the `host` and the `target` platform.

The supported variables are: `PKG_CONFIG_PATH`, `PKG_CONFIG_LIBDIR`, and
`PKG_CONFIG_SYSROOT_DIR`.

Each of these variables can also be supplied with certain prefixes and suffixes, in the following prioritized order:

1. `<var>_<target>` - for example, `PKG_CONFIG_PATH_x86_64-unknown-linux-gnu`
2. `<var>_<target_with_underscores>` - for example, `PKG_CONFIG_PATH_x86_64_unknown_linux_gnu`
3. `<build-kind>_<var>` - for example, `HOST_PKG_CONFIG_PATH` or `TARGET_PKG_CONFIG_PATH`
4. `<var>` - a plain `PKG_CONFIG_PATH`

Also note that `PKG_CONFIG_ALLOW_CROSS` must always be set in cross-compilation context.

# License

This project is licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or
   http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in Serde by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
