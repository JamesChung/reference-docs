# Rust Reference

## Visibility Modifiers

<https://doc.rust-lang.org/reference/visibility-and-privacy.html>

`pub(crate)`

`pub(in path)`

## Annotations

- `#[must_use]`: Add it to any type, trait, or function, and the compiler will
  issue a warning if the user's code receives and element of that type or trait,
  or calls that function, and does not explicitly handle it.
- `#[non_exhaustive]`: You can add it to any type definition, and the compiler
  will disallow the use of implicit constructors and non-exhaustive pattern
  matches(that is, patterns without a trailing, `..`) on that type.

## Attributes

- `#[cfg(feature = "some-feature")]`: makes it so that the next "thing" in the
  source code is compiled only if the `some-feature` feature is enabled.
  Similarly, `if cfg!(feature = "some-feature")` is equivalent to `if true` only
  if the `derive` feature is enabled (and `if false` otherwise).
  - You can place `#[cfg]` in front of certain Rust _items_ such as functions
    and type definitions, `impl` blocks, modules, and `use` statements - as well
    as on certain other contructs like struct fields, function arguements, and
    statements.

## Documentation

Consider marking parts of your interface that are not intended to be public but
are needed for legacy reasons with `#[doc(hidden)]`, so that they do not clutter
up your documentation.

Use `#[doc(cfg(..))]` to highlight items that are only available under certain
configurations so the user quickly realizes why some method that's listed in the
documentation isn't available.

Use `#[doc(alias = "...")]` to make types and methods discoverable under other
names that users may search for them by.

In the top-level documentation, point the user to commonly used modules,
features, types, traits, and methods.

## Creates vs. Packages

A create is a Rust module hierarchy starting at a root _.rs_ file (one where you
can use crate-level attributes like `#![feature]`) - usually something like
_lib.rs_ or _main.rs_.

In contrast, a package is a collection of crates and metadata, so essentially
all that's described by a _Cargo.toml_ file. That may include a library crate,
multiple binary crates, some integration test crates, and maybe even multiple
workspace members that themselves have _Cargo.toml_ files.

## The SemVer Trick

The itercrate example may have rubbed you the wrong way. If the Empty type did
not change, then why does the compiler not allow anything that uses it to keep
working, regardless of whether the code is using version 1.0 or 2.0 of it? The
answer is . . . complicated. It boils down to the fact that the Rust compiler
does not assume that just because two types have the same fields, they are the
same. To take a simple example of this, imagine that itercrate 2.0 added a
#[derive(Copy)] for Empty. Now, the type suddenly has different move semantics
depending on whether you are using 1.0 or 2.0! And code written with one in mind
won’t work with the other.

This problem tends to crop up in large, widely used libraries, where over time,
breaking changes are likely to have to happen somewhere in the crate.
Unfortunately, semantic versioning happens at the crate level, not the type
level, so a breaking change anywhere is a breaking change everywhere.

But all is not lost. A few years ago, David Tolnay (the author of serde, among a
vast number of other Rust contributions) came up with a neat trick to handle
exactly this kind of situation. He called it “the semver trick.” The idea is
simple: if some type T stays the same across a breaking change (from 1.0 to 2.0,
say), then after releasing 2.0, you can release a new 1.0 minor version that
depends on 2.0 and replaces T with a re-export of T from 2.0.

By doing this, you’re ensuring that there is in fact only a single type T across
both major versions. This, in turn, means that any crate that depends on 1.0
will be able to use a T from 2.0, and vice versa. And because this happens only
for types you explicitly opt into with this trick, changes that were in fact
breaking will continue to be.

## The Never Type

Some functions, like those that start a continuously running server loop, only
ever return errors; unless an error occurs, they run forever. Other functions
never error but need to return a `Result` nonetheless, for example, to match a
trait signature. For functions like these , Rust provides the _never type_,
written with the `!` syntax.

The never type represents a value that can never be generated. You cannot
construct an instance of this type yourself - the only way to make one is by
entering an infinite loop or panicking, or through a handful of other special
operations that the compiler knows never return.

With `Result`, when you have an `Ok` or `Err` that you know will never be used,
you can set it to the `!` type. If you write a function that returns
`Result<T, !>`, you will be unable to ever return `Err`, since the only way to
do so is to enter code that will never return. Because the compiler knows that
any variant with `!` will never be produced, it can also optimize your code with
that in mind, such as by not generating the panic code for an `unwrap` on
`Result<T, !>`. And when you pattern match, the cimpiler knows that any variant
that contains a `!` does not even need to be listed.

## Common Traits for Types

- `Debug`
  - Nearly every type can, and should, implement `Debug`, even if it only prints
    the type's name.
  - Using `#[derive(Debug)]` is often the best way to implement the `Debug`
    trait in your interface, keep in mind that all derived traits automatically
    add the same bound for any generic parameters.
  - You can also write your own implementation by leveraging the various
    `debug_` helpers on `fmt::Formatter`.
- `Send`
  - A type that is not `Send` can't be placed in a `Mutex` and can't be used
    even transitively in an application that contains a thread pool.
- `Sync`
  - A type that is not `Sync` can't be shared through an `Arc` or placed in a
    static variable.
- `Unpin`
- `Clone`
- `Default`
- `PartialEq`
  - Desirable because users will at some point inevitably have two instances of
    your type that they wish to compare with `==` or `assert_eq!`.
  - Even if your type would compare equal for only the same instance of the
    type, it's worth implementing `PartialEq` to enable your users to use
    `assert_eq!`.
- Specialized Traits (should only implement when needed/applicable for your
  type)
  - `PartialOrd`
  - `PartialHash`
  - `Eq`
  - `Ord`
- `serde`
  - `Serialize`
  - `Deserialize`

> Users do not generally expect types to be `Copy`; quite to the contrary, they
> tend to expect that if they want two copies of something, they have to call
> `clone`. `Copy` changes the semantics of moving a value of the given type,
> which might surprise the user.

## Ergonomic Trait Implementations

When you define a new trait, you'll usually want to provide blanket
implementations as appropriate for that trait for `&T where T: Trait`,
`&mut T where T: Trait`, and `Box<T> where T: Trait`. You may be able to
implement only some of these depending on what receivers the methods of `Trait`
have. Many of the traits in the standard library have similar implementations,
precisely because that leads to fewer surprises for the user.

For any type that can be iterated over, consider implementing `IntoIterator` for
both `&MyType` and `&mut MyType` where applicable. This makes `for` loops work
with borrowed instances of your type as well out of the box, just like users
would expect.

## Wrapper Types

- `Deref`
- `AsRef`
- `From`
- `Into`
- `Borrow`

If you provide a relatively transparent wrapper type (like `Arc`), there's a
good chance you'll want to implement `Deref` so that users can call methods on
the inner type by just using the `.` operator.

If accessing the inner type does not require any complex or potentially slow
logic, you should also consider implementing `AsRef`, which allows users to
easily use a `&WrapperType` as an `&InnerType`.

For most wrapper types, you will also want to implement `From<InnerType>` and
`Into<InnerType>` where possible so that your users can easily add or remove
your wrapping.

`Borrow` allows the caller to supply any one of multiple essentially identical
variants of the same type. For example, for a `HashSet<String>`, `Borrow` allows
the caller to supply either a `&str` or a `&String`. While the same could have
been achieved with `AsRef`, that would not be safe without `Borrow`'s additional
requirement that the target type implements `Hash`, `Eq`, and `Ord` exactly the
same as the implementing type. `Borrow` also has a blanket implementation of
`Borrow<T>` for `T`, `&T`, and `&mut T`, which makes it convenient to use in
trait bounds to accept either owned or referenced values of a given type. In
general, `Borrow` is intended only for when your type is essentially equivalent
to another type, whereas `Deref` and `AsRef` are intended to be implemented more
widely for anything your type can "act as".

Since Rust needs to be able to drop trait objects, every vtable contains the
`drop` method. Effectively, every `dyn Trait` is also a `dyn Drop`.

## Type System Guidance

Semantic Typing, which adds types to represent the _meaning_ of a value, not
just its primitive type. The classic example is if your function takes three
`bool` arguments, chances are some user will mess up the order of the values.
However if it takes trhee arguments of distinct two-variant enum types, the user
cannot get the order wrong without the compiler yelling at them: if they attempt
to pass `DryRun::Yes` to the `overwrite` argument, that will simply not work,
not will passing `Overwrite::No` as the `dry_run` argument.

A closely related technique is to use zero-sized types to indicate that a
particular fact is true about an instance of a type.

> We don't actually need to store the stage - only the meta-information it
> provides so we store it behind a `PhantomData` to guarantee that it is
> eliminated at compile time.

```rust
struct Grounded;
struct Launched;
// and so on...

struct Rocket<Stage = Grounded> {
    stage: std::marker::PhantomData<Stage>,
}

impl Default for Rocket<Grounded> {}

impl Rocket<Grounded> {
    pub fn launch(self) -> Rocket<Launched> {}
}

impl Rocket<Launched> {
    pub fn accelerate(&mut self) {}
    pub fn decelerate(&mut self) {}
}

impl<Stage> Rocket<Stage> {
    pub fn color(&self) -> Color {}
    pub fn weight(&self) -> Kilograms {}
}
```

## Errors

When making your own error type, you need to take a number of steps to make the
error type play nicely with the rest of the Rust ecosystem.

1. Your error type should implement the `std::error::Error` trait, which
   provides callers with common methods for introspecting error types. The main
   method if interest is `Error::source`, which provides a mechanism to find the
   underlying cause of an error. This is most commonly used to print a backtrace
   that displays a trace all the way back to the error's root cause.
2. Your type should implement both `Display` and `Debug` so that callers can
   meaningfully print your error. This is required if you implement the `Error`
   trait. In general, your implementation of `Display` should give a one-line
   description of what went wrong that can easily be folded into other error
   messages. The display format should be lowercase and without trailing
   punctuation so that it fits nicely into other, larger error reports. `Debug`
   should provide a more descriptive error including auxiliary information that
   may be useful in tracking down the cause of the error, such as port numbers,
   request identifiers, filepaths, and the like, which `#[derive(Debug)]` is
   usually sufficient for.
3. Your type should, if possible, implement both `Send` and `Sync` so that users
   are able to share the error across thread boundaries. If your error type is
   not thread-safe, you will find that it's almost impossible to use your crate
   in a multithreaded context. Error types that implement `Send` and `Sync` are
   also much easier to use with the very common `std::io::Error` type, which is
   able to wrap errors that implement `Error`, `Send`, and `Sync`.
4. Where possible, your error type should be `'static`.

> In general, the community consensus is that errors should be rare and
> therefore should not add much cost to the "happy path". For that reason,
> errors are often placed behind a pointer type, such as a `Box` or `Arc`. This
> way, they're unlikely to add much to the size of the overall `Result` type
> they're contained within.

---

> While `Box<dyn Error + ...>` is an attractive type-erased error type, it
> counterintuitively does not itself implement `Error`. Therefore, consider
> adding your own `BoxError` type for type erasure in libraries that does
> implement `Error`.

---

> Keep in mind that `()` does not implement the `Error` trait. This means that
> it cannot be type-erased into `Box<dyn Error>` and can be a bit of a pain to
> use with `?`. For this reason, it is often better to define your own unit
> struct type, implement `Error` for it, and use that as the error instead of
> `()` in these cases.

### Propagating Errors

Rust's `?` operator acts as a shorthand for _unwrap or return early_, for
working easily with errors. But it also has a few other tricks up its sleeve
that are worth knowing about.

The `?` operator at the time of writing uses `From`, not `Into`.

1. `?` performs type conversion through the `From` trait. In a function that
   returns `Result<T, E>`, you can use `?` on any `Result<T, X>` where
   `E: From<X>`. Under the hood, the `?` operator calls `From::from` on the
   error value to convert it to a boxed trait object, a `Box<dyn error::Error>`,
   which is polymorphic-- that means that lots of different kinds of errors can
   be returned from the same function because all errors act the same since they
   all implement the `error::Error` trait.
2. `?` operator is really just syntax sugar for a trait tentatively called `Try`

Hint: There are two different possible `Result` types produced within `main()`,
which are propagated using `?` operators. How do we declare a return type from
`main()` that allows both?

Check out this section of the book:
<https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator>

Read more about boxing errors:
<https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/boxing_errors.html>

Read more about using the `?` operator with boxed errors:
<https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html>

## Project Structure

### Cargo

#### Defining and Including Features

Features are defined in `Cargo.toml`.

```toml
[package]
name = "foo"
...
[features]
derive = ["syn"]

[dependencies]
syn = { version = "1", optional = true }
```

When Cargo compiles this crate, it will not compile the `syn` crate by default,
which reduces compile time (often significantly). The `syn` crate will be
compiled only if a downstream crate needs to use the APIs enabled by the
`derive` feature and explicitly opts in to it.

> How such a downstream crate `bar` would enable the `derive` feature, and thus
> include the `syn` dependency.

```toml
[package]
name = "bar"
...
[dependencies]
foo = { version = "1", features = ["derive"] }
```

Some features are used so frequently that it makes more sense to have a crate
opt out of them rather than in to them. To support this, Cargo allows you to
define a set of default features for a crate. And similarly, it allows you to
opt out of the default features of a dependency.

> How `foo` can make its `derive` feature enabled by default, while also opting
> out of some of `syn`'s default features and instead enabling only the ones it
> needs for the `derive` feature.

```toml
[package]
name = "foo"
...
[features]
derive = ["syn"]
default = ["derive"]

[dependencies.syn]
version = "1"
default-features = false
features = ["derive", "parsing", "printing"]
optional = true
```

Here, if a create depends on `foo` and does not explicitly opt out of the
default features, it will also compile `foo`'s `syn` dependency. In turn, `syn`
will be built with only the three listed features, and no others. Opting out of
default features this way, and opting in to only what you need, is a great way
to cut down on your compile times!

> If you're writing a large crate where you expect that your users will need
> only a subset of the functionality, you should consider making it so that
> larger components (usually modules) are guarded by features. That way, users
> can opt in to, and pay the compilation cost of, only the parts they really
> need.

#### Workspaces

A _workspace_ is a collection of crates (often called _subcrates_) that are tied
together by a top-level `Cargo.toml` file.

```toml
[workspace]
members = [
  "foo",
  "bar/one",
  "bar/two",
]
```

The `members` array is a list of directories that each contain a crate in the
workspace. Those creates all have their own `Cargo.toml` files in their own
subdirectories, but they share a single `Cargo.lock` file and a single output
directory.

The create names don't need to match the entry in `members`. It is common, but
not required, that crates in a workspace share a name prefix, usually chosen as
the name of the "main" create. For example, in the `tokio` crate, the members
are called `tokio`, `tokio-test`, `tokio-macros`, and so on.

Perhaps the most important feature of workspaces is that you can interact with
all of the workspace's members by invoking `cargo` in the root of the workspace.
Want to check that they all compile? `cargo check` will check them all. Want to
run all your tests? `cargo test` will test them all. It's not quite as
convenient as having everything in one crate, so don't go splitting everything
into minuscule crates, but it's a pretty good approximation.

> Cargo commands will generally do the "right thing" in a workspace. If you ever
> need to disambiguate, such as if two workspace crates both have a binary by
> the same name, use the `-p` flag (for package). If you are in the subdirectory
> for a particular workspace crate, you can pass `--workspace` to perform the
> command for the entire workspace instead.

Once you have a workspace-level `Cargo.toml` with the array of workspace
members, you can set your crates to depend on one another using path
dependencies.

```toml
# bar/two/Cargo.toml
[dependencies]
one = { path = "../one" }
# bar/one/Cargo.toml
[dependencies]
foo = { path = "../../foo" }
```

Now if you make a change to the crate in _bar/two_, then only that crate is
re-compiled, since `foo` and _bar/one_ did not change. It may even be faster to
compile your project from scratch, since the compiler does not need to evaluate
your entire project source for optimization opportunities.

## Project Configuration

### Crate Metadata

For crates with a more convoluted project layout, it's also useful to set the
`include` and `exclude` metadata fields. These dictate which files should be
included and published in your package.

**By default, Cargo includes all files in a crate's directory except any listed
in your `.gitignore` file.**

You may not want this if you also have large test fixtures, unrelated scripts,
or other auxiliary data in the same directory that you _do_ want under version
control.

As their names suggest, `include` and `exclude` allow you to include only a
specific set of files or exclude files matching a given set of patterns,
respectively.

> If you have a create that should never be published, or should be published
> only to certain alternative registries (that is, not to crates.io), you can
> set the `publish` directive to `false` or to a list of allowed registries.

[Metadata Directives Reference](https://doc.rust-lang.org/cargo/reference/manifest.html)

### Build Configuration

`Cargo.toml` can give you control over how Cargo builds your crate.

#### [patch]

The `[patch]` section of _Cargo.toml_ allows you to specify a different source
for a dependency that you can use temporarily, no matter where in your
dependencies the patched dependency appears.

This is invaluable when you need to compile your crate against a modified
version of some transitive dependency to test a bug fix, a performance
improvement, or a new minor release you're about to publish.

```toml
[patch.crates-io]
# use a local (presumably modified) source
regex = { path = "/home/james/regex" }
# use a modification on a git branch
serde = { git = "https://github.com/serde-rs/serde.git", branch = "faster" }
# patch a git dependency
[patch.'https://github.com/jonhoo/project.git']
project = { path = "/home/james/project" }
```

Even if you patch a dependency, Cargo takes care to check the crate versions so
that you don't accidently end up patching the wrong major version of a crate. If
you for some reason transitively depend on multiple major versions of the same
crate, you can patch each one by giving them distinct identifiers.

```toml
[patch.crates-io]
nom4 = { path = "/home/james/nom4", package = "nom" }
nom5 = { path = "/home/james/nom5", package = "nom" }
```

Cargo will look at the _Cargo.toml_ inside each path, realize that `/nom4`
contains major version 4 and that `/nom5` contains major version 5, and patch
the two versions appropriately. The `package` keyword tells Cargo to look for a
crate by the name `nom` in both cases instead of using the dependency
identifiers (the part on the left) as it does by default. You can use `package`
this way in your regular dependencies as well to rename a dependency.

Keep in mind that patches are not taken into account in the package that's
uploaded when you publish a crate. A crate that depends on your crate will use
only it's own `[patch]` section (which may be empty), not that of your crate!

#### [profile]

The `[profile]` section lets you pass additional options to the Rust compiler in
order to change the way it compiles your crate. These options fall primarily
into three categories:

- performance options
- debugging options
- options that change code behavior in user-defined ways

They all have different defaults depending on whether you are compiling in debug
mode or in release mode (other modes also exist).

The three primary performance options are:

- `opt-level`
- `codegen-units`
- `lto`

---

The `opt-level` option tweaks runtime performance by telling the compiler how
aggressively to optimize your program (0 is "not at all", 3 is "as much as you
can"). The higher the setting, the more optimized your code will be, which _may_
make it run faster. Extra optimization comes at the cost of higher compile
times, which is why optimizations are generally enabled only for release builds.

> You can also set `opt-level` to _"s"_ to optimize for binary size, which may
> be important on embedded platforms.

---

The `codegen-units` option is about compile-time performance. It tells the
compiler how many independent compilation tasks (_code generation units_) it is
allowed to split the compilation of a single crate into. The more pieces a large
crate's compilation is split into, the faster it will compile, since more
threads can help compile the crate in parallel. **Unfortunately, to achieve this
speedup, the threads need to work more or less independently, which means code
optimization suffers.** Imagine, for example, that the segment of a create
compiling in one thread could benefit from inlining some code in a different
segment - since the two segments are independent, that inlining can't happen!
This setting, then, is a trade-off between compile-time performance and runtime
performance.

By default, Rust uses an effectively unbounded number of codegen units in debug
mode (basically, "compile as fast as you can") and a smaller number (16 at the
time of writing) in release mode.

---

The `lto` setting toggles _link-time optimization (LTO)_, which enables the
compiler/linker to jointly optimize bits of your program, known as _compilation
units_, that were originally compiled separately. The output from each
compilation unit includes information about the code that went into that unit.
After all the units have been compiled, the linker makes another pass over all
of the units and uses that additional information to optimize the combined
compiled code. This extra pass adds to the compile time but recovers most of the
runtime performance that may have been lost due to splitting the compilation
into smaller parts. In particular, LTO can offer significant performance boosts
to performance-sensitive programs that might benefit from cross-crate
optimization. **Beware, cross-crate LTO can add a lot to your compile time.**

Rust performs LTO across all the codegen units within each crate by default in
an attempt to make up for the lost optimizations caused by using many codegen
units. Since the LTO is performed only within each crate, rather than across
crates, this extra pass isn't too onerous, and the added compile time should be
lower than the amount of time saved by using a lot of codegen units. Rust also
offers a technique known as _thin LTO_, which allows the LTO pass to be mostly
parallelized, at the cost of missing some optimizations a "full" LTO pass would
have found.

> LTO can be used to optimize across foreign function interface boundaries in
> many cases, too. See the `linker-plugin-lto` `rustc` flag for more details.

---

When a thread panics and unwinds, other threads continue running unaffected.
Only when (and if) the thread that ran `main` exits does the program terminate.
That is, the panic is generally isolated to the thread in which the panic
occurred.

> The panic setting is global - if you set it to `abort`, all your dependencies
> are also compiled with `abort`.

You can have backtraces even with `panic=abort` by passing
`-Cforce-unwind-tables` to `rustc`, which makes `rustc` include the information
necessary to walk back up the stack while still terminating the program on a
panic.

#### Profile Overrides

You can set profile options for just a particular dependency, or a particular
profile, using _profile overrides_.

Enable aggressive optimizations for the `serde` crate and moderate optimizations
for all other crates in debug mode, using the
`[profile.<profile-name>.package.<crate-name>]` syntax.

```toml
[profile.dev.package.serde]
opt-level = 3
[profile.dev.package."*"]
opt-level = 2
```

You can also specify global profile defaults using a `[profile.dev]` (or
similar) section in the Cargo configuration file in _~/.cargo/config_

When you set optimization parameters for a specific dependency, keep in mind
that the parameters apply only to the code compiled as part of that crate; if
`serde` in this example has a generic method or type that you use in your crate,
the code of that method or type will be monomorphized and optimized in your
create, and your crate's profile settings will apply, not those in the profile
override for `serde`.

### Conditional Compilation

To conditionally compile sections of your code you can use the `cfg` keyword.
It's usually in the form of `#[cfg(condition)]`, which says to compile the next
item only if `condition` is true.

Rust also has `#[cfg_attr(condition, attribute)]` which is compiled as
`#[attribute]` if `condition` holds and is a no-op otherwise.

You can also evaluate a `cfg` condition as a Boolean expression using the
`cfg!(condition)` macro.

Every `cfg` construct takes a single condition made up of options, like
`feature = "some-feature"`, and the combinators `all`, `any`, and `not`. Options
are either simple names, like `unix`, or key/value pairs like those used by
feature conditions.

There are a number of interesting options you can make compilation depend on.

#### Feature options

Feature options take the form `feature = "name-of-feature"` and are considered
true if the name feature is enabled. You can check for multiple features in a
single condition using the combinators. For example,
`any(feature = "f1", feature = "f2")` is true if either feature `f1` or feature
`f2` is enabled.

#### Operating system options

These use key/value syntax with the key `target_os` and values like `windows`,
`macos`, and `linux`. You can also specify a family of operating systems using
`target_family`, which takes the value `windows` or `unix`. These are common
enough that they have received their own named short forms, so you can use
`cfg(windows)` and `cfg(unix)` directly. For example, if you wanted a particular
code segment to be compiled only on macOS and Windows, you would write:
`#[cfg(any(windows, target_os = "macos"))]`

#### Context options

These let you tailor code to a particular compilation context. The most common
of these is the `test` option, which is true only when the crate is being
compiled under the test profile. Keep in mind that `test` is set only for the
crate that is being tested, not for any of its dependencies. This also means
that `test` is not set in your crate when running integration tests; it's the
integration tests that are compiled under the test profile, whereas your actual
crate is compiled normally (that is, without `test` set). The same applies to
the `doc` and `doctest` options, which are set only when building documentation
or compiling doctests, respectively. There's also the `debug_assertions` option,
which is set in debug mode by default.

#### Tool options

Some tools, like clippy and Miri, set custom options that let you customize
compilation when run under these tools. Usually, these options are named after
the tool in question. For example, if you want a particular compute-intensive
test not to run under Miri, you can give it the attribute
`#[cfg_attr(miri, ignore)]`.

#### Architecture options

These let you compile based on the CPU instruction set the compiler is
targeting. You can specify a particular architecture with `target_arch`, wich
takes values like `x86`, `mips`, and `aarch64`, or you can specify a particular
platform feature with `target_feature`, wich takes values like `avx` or `sse2`.
For very low-level code, you may also find the `target_endian` and
`target_pointer_width` options useful.

#### Compiler options

These let you adapt your code to the platform ABI (Application Binary Interface)
it is compiled against and are available through `target_env` with values like
`gnu`, `msvc`, and `musl`. FOr historical reasons, this value is often empty,
especially on GNU platforms. You normally need this option only if you need to
interface directly with the environment ABI, such as when linking against an
ABI-specific symbol name using `#[link]`.

> Here, we specify that `winrt` should be considered a dependency only under
> windows and `nix` only under unix.

```toml
[target.'cfg(windows)'.dependencies]
winrt = "0.7"
[target.'cfg(unix).dependencies]
nix = "0.17"
```

> Recommendation: set up your CI infrastructure to perform basic auditing of
> your dependencies using tools like `cargo-deny` and `cargo-audit`. These tools
> will detect cases where you transitively depend on multiple major versions of
> a given dependency, where you depend on crates that are unmaintained or have
> known security vulnerabilities, or where you use licenses that you may want to
> avoid. Using such a tool is a great way to raise the quality of your codebase
> in an automated way!

### Versioning

Minimum supported Rust version (MSRV)

There are two techniques crate authors can use to make life a little easier for
users.

1. Establish an MSRV policy promising that new versions of a crate will always
   compile with any stable release from the last _X_ months. The exact number
   varies, but 6 or 12 months is common.
2. Make sure to increase the minor version number of your crate any time that
   the MSRV changes. So, if you release version 2.7.0 of your crate and that
   increases your MSRV from Rust 1.44 to Rust 1.45, then a project that is stuck
   on 1.44 and that depends on your crate can use the dependency version
   specifier `version = "2, <2.7"` to keep the project working until it can move
   on to Rust 1.45. It's important that your increment the minor version, not
   just the patch version, so that you can still issue critical security fixes
   for the previous MSRV release by doing another patch release if necessary.

## Testing

When you run `cargo test --lib`, the only special thing Cargo does is pass the
`--test` flag to `rustc`. This flag tells `rustc` to produce a test binary that
runs all the unit tests, rather than just compiling the crate's library or
binary.

Behind the scenes, `--test` has two primary effects.

1. It enables `cfg(test)` so that you can conditionally include testing code.
2. It makes the compiler generate a _test harness_: a carefully generated `main`
   function that invokes each `#[test]` function in your program when it's run.

### The Test Harness

The compiler generates the test harness `main` function through a mix of
procedural macros.

Essentially, the harness transforms every function annotated by `#[test]` into a
test _descriptor_ - this is the procedural macro part. It then exposes the path
of each of the descriptors to the generated `main` function. The descriptor
includes information like the test's name, any additional options it has set
(like `#[should_panic]`), and so on. At its core, the test harness iterates over
the tests in the crate, runs them, captures their results, and prints the
results.

Integration tests (the tests in `tests/`) follow the same process as unit tests,
with the one exception that they are each compiled as their own separate crate,
meaning they can access only the main crate's public interface and are run
against the main crate compiled without `#[cfg(test)]`. A test harness is
generated for each file in `tests/`. Test harnesses are not generated for files
in subdirectories under _tests/_ to allow you to have shared submodules for your
tests.

> If you explicitly want a test harness for a file in a subdirectory, you can
> opt in to that by calling the file main.rs

Rust does not require that you use the default test harness. You can instead opt
out of it and implement your own `main` method that represents the test runner
by setting `harness = false` for a given integration test in _Cargo.toml_

```toml
[[test]]
name = "custom"
path = "tests/custom.rs"
harness = false
```

Without the test harness, none of the magic around `#[test]` happens. Instead,
you're expected to write your own `main` function to run the testing code you
want to execute. Essentially, you're writing a normal Rust binary that just
happens to be run by `cargo test`. That binary is responsible for handling all
the things that the default harness normally does (if you want to support them),
such as command line flags. The `harness` property is set separately for each
integration test, so you can have one test file that uses the standard harness
and one that does not.

### Doctests

Rust code snippets in documentation comments are automatically run as test
cases. Because doctests appear in the public documentation of your crate, and
users are likely to mimic what they contain, they are run as integration tests.
This means that the doctests don't have access to private fields and methods,
and `test` is not set on the main crate's code. Each doctest is compiled as its
own dedicated crate and is run in isolation, just as if the user had copy-pasted
the doctest into their own program.

Behind the scenes, the compiler performs some preprocessing on doctests to make
them more concise. Most importantly, it automatically adds an `fn main` around
your code. This allows doctests to focus only on the important bits that the
user is likely to care about, like the parts that actually use types and methods
from your library, without including unnecessary boilerplate.

You can opt out of this auto-wrapping by defining your own `fn main` in the
doctest. You may want to do this, for example, if you want to write an
asynchronous `main` function using something like
`#[tokio::main] async fn main`, or if you want to add additional modules to the
doctest.

To use the `?` operator in your doctest, you don't normally have to use a custom
`main` function as `rustdoc` includes some heuristics to set the return type to
`Result<(), impl Debug>` if your code looks like it makes use of `?` (for
example, if it ends with `Ok(())`). If type inference gives you a hard time
about the error type ofr the function, you can disambiguate it by changing the
last line of the doctest to be explicitly typed, like this: `Ok::<(), T>()`.

Doctests have a number of additional features that come in handy as you write
documentation for more complex interfaces. The first is the ability to hide
individual lines. If you prefix a line of a doctest with a `#`, that line is
included when the doctest is compiled and run, but it is not included in the
code snippet generated in the documentation. This lets you easily hide details
that are not important to the current example, such as implementing traits for
dummy types or generating values. it is also useful if you wish to present a
sequence of examples without showing the same leading code each time.

````rust
/// Completely frobnifies a number though I/O
///
/// In this first example we hide the value generation.
/// ```
/// # let unfrobnified_number = 0;
/// # let already_frobnified = 1;
/// assert!(frobnify(unfrobnified_number).is_ok());
/// assert!(frobnify(already_frobnified).is_err());
/// ```
///
/// Here's an example that uses ? on multiple types
/// and thus needs to declare the concrete error type,
/// but we don't want to distract the user with that.
/// We also hide the use that brings the function into scope.
/// ```
/// # use mylib::frobnify;
/// frobnify("0".parse()?)?;
/// # Ok::<(), anyhow::Error>(())
/// ```
///
///You could even replace an entire block of code completely,
/// though use this very sparingly:
/// ```
/// # /*
/// let i = ...;
/// # */
/// # let i = 42;
/// frobnify(i)?;
/// ```
fn frobnify(i: usize) -> std::io::Result<()> { ...
````

> Much like `#[test]` functions, doctests also support attributes that modify
> how the doctest is run. These attributes go immediately after the
> tripple-backtick used to denote a code block, and miltiple attributes can be
> separated by commas.

Like with test functions, you can specify the `should_panic` attribute to
indicate that the code in a particular doctest should panic when run, or
`ignore` to check the code segment only if `cargo test` is run with the
`--ignored` flag. You can also use the `no_run` attribute to indicate that a
given doctest should compile but should not be run.

```compile_fail
# struct MyNonSendType(std::rd::Rc<()>);
fn is_send<T: Send>() { ... }
is_send::<MyNonSendType>();
```
