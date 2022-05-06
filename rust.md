# Rust Reference

## Visibility Modifiers

`pub(crate)`

`pub(in path)`

## Annotations

- `#[must_use]`: Add it to any type, trait, or function, and the compiler will issue a warning if the user's code receives and element of that type or trait, or calls that function, and does not explicitly handle it.
- `#[non_exhaustive]`: You can add it to any type definition, and the compiler will disallow the use of implicit constructors and non-exhaustive pattern matches(that is, patterns without a trailing, `..`) on that type.

## Documentation

Consider marking parts of your interface that are not intended to be public but are needed for legacy reasons with `#[doc(hidden)]`, so that they do not clutter up your documentation.

Use `#[doc(cfg(..))]` to highlight items that are only available under certain configurations so the user quickly realizes why some method that's listed in the documentation isn't available.

Use `#[doc(alias = "...")]` to make types and methods discoverable under other names that users may search for them by.

In the top-level documentation, point the user to commonly used modules, features, types, traits, and methods.

## The SemVer Trick

The itercrate example may have rubbed you the wrong way. If the Empty type did not change, then why does the compiler not allow anything that uses it to keep working, regardless of whether the code is using version 1.0 or 2.0 of it? The answer is . . . complicated. It boils down to the fact that the Rust compiler does not assume that just because two types have the same fields, they are the same. To take a simple example of this, imagine that itercrate 2.0 added a #[derive(Copy)] for Empty. Now, the type suddenly has different move semantics depending on whether you are using 1.0 or 2.0! And code written with one in mind won’t work with the other.

This problem tends to crop up in large, widely used libraries, where over time, breaking changes are likely to have to happen somewhere in the crate. Unfortunately, semantic versioning happens at the crate level, not the type level, so a breaking change anywhere is a breaking change everywhere.

But all is not lost. A few years ago, David Tolnay (the author of serde, among a vast number of other Rust contributions) came up with a neat trick to handle exactly this kind of situation. He called it “the semver trick.” The idea is simple: if some type T stays the same across a breaking change (from 1.0 to 2.0, say), then after releasing 2.0, you can release a new 1.0 minor version that depends on 2.0 and replaces T with a re-export of T from 2.0.

By doing this, you’re ensuring that there is in fact only a single type T across both major versions. This, in turn, means that any crate that depends on 1.0 will be able to use a T from 2.0, and vice versa. And because this happens only for types you explicitly opt into with this trick, changes that were in fact breaking will continue to be.

## The Never Type

Some functions, like those that start a continuously running server loop, only ever return errors; unless an error occurs, they run forever. Other functions never error but need to return a `Result` nonetheless, for example, to match a trait signature. For functions like these , Rust provides the _never type_, written with the `!` syntax.

The never type represents a value that can never be generated. You cannot construct an instance of this type yourself - the only way to make one is by entering an infinite loop or panicking, or through a handful of other special operations that the compiler knows never return.

With `Result`, when you have an `Ok` or `Err` that you know will never be used, you can set it to the `!` type. If you write a function that returns `Result<T, !>`, you will be unable to ever return `Err`, since the only way to do so is to enter code that will never return. Because the compiler knows that any variant with `!` will never be produced, it can also optimize your code with that in mind, such as by not generating the panic code for an `unwrap` on `Result<T, !>`. And when you pattern match, the cimpiler knows that any variant that contains a `!` does not even need to be listed.

## Common Traits for Types

- `Debug`
  - Nearly every type can, and should, implement `Debug`, even if it only prints the type's name.
  - Using `#[derive(Debug)]` is often the best way to implement the `Debug` trait in your interface, keep in mind that all derived traits automatically add the same bound for any generic parameters.
  - You can also write your own implementation by leveraging the various `debug_` helpers on `fmt::Formatter`.
- `Send`
  - A type that is not `Send` can't be placed in a `Mutex` and can't be used even transitively in an application that contains a thread pool.
- `Sync`
  - A type that is not `Sync` can't be shared through an `Arc` or placed in a static variable.
- `Unpin`
- `Clone`
- `Default`
- `PartialEq`
  - Desirable because users will at some point inevitably have two instances of your type that they wish to compare with `==` or `assert_eq!`.
  - Even if your type would compare equal for only the same instance of the type, it's worth implementing `PartialEq` to enable your users to use `assert_eq!`.
- Specialized Traits (should only implement when needed/applicable for your type)
  - `PartialOrd`
  - `PartialHash`
  - `Eq`
  - `Ord`
- `serde`
  - `Serialize`
  - `Deserialize`

> Users do not generally expect types to be `Copy`; quite to the contrary, they tend to expect that if they want two copies of something, they have to call `clone`. `Copy` changes the semantics of moving a value of the given type, which might surprise the user.

## Ergonomic Trait Implementations

When you define a new trait, you'll usually want to provide blanket implementations as appropriate for that trait for `&T where T: Trait`, `&mut T where T: Trait`, and `Box<T> where T: Trait`. You may be able to implement only some of these depending on what receivers the methods of `Trait` have. Many of the traits in the standard library have similar implementations, precisely because that leads to fewer surprises for the user.

For any type that can be iterated over, consider implementing `IntoIterator` for both `&MyType` and `&mut MyType` where applicable. This makes `for` loops work with borrowed instances of your type as well out of the box, just like users would expect.

## Wrapper Types

- `Deref`
- `AsRef`
- `From`
- `Into`
- `Borrow`


If you provide a relatively transparent wrapper type (like `Arc`), there's a good chance you'll want to implement `Deref` so that users can call methods on the inner type by just using the `.` operator.

If accessing the inner type does not require any complex or potentially slow logic, you should also consider implementing `AsRef`, which allows users to easily use a `&WrapperType` as an `&InnerType`.

For most wrapper types, you will also want to implement `From<InnerType>` and `Into<InnerType>` where possible so that your users can easily add or remove your wrapping.

`Borrow` allows the caller to supply any one of multiple essentially identical variants of the same type. For example, for a `HashSet<String>`, `Borrow` allows the caller to supply either a `&str` or a `&String`. While the same could have been achieved with `AsRef`, that would not be safe without `Borrow`'s additional requirement that the target type implements `Hash`, `Eq`, and `Ord` exactly the same as the implementing type. `Borrow` also has a blanket implementation of `Borrow<T>` for `T`, `&T`, and `&mut T`, which makes it convenient to use in trait bounds to accept either owned or referenced values of a given type. In general, `Borrow` is intended only for when your type is essentially equivalent to another type, whereas `Deref` and `AsRef` are intended to be implemented more widely for anything your type can "act as".

Since Rust needs to be able to drop trait objects, every vtable contains the `drop` method. Effectively, every `dyn Trait` is also a `dyn Drop`.

## Type System Guidance

Semantic Typing, which adds types to represent the _meaning_ of a value, not just its primitive type. The classic example is if your function takes three `bool` arguments, chances are some user will mess up the order of the values. However if it takes trhee arguments of distinct two-variant enum types, the user cannot get the order wrong without the compiler yelling at them: if they attempt to pass `DryRun::Yes` to the `overwrite` argument, that will simply not work, not will passing `Overwrite::No` as the `dry_run` argument.

A closely related technique is to use zero-sized types to indicate that a particular fact is true about an instance of a type.

> We don't actually need to store the stage - only the meta-information it provides so we store it behind a `PhantomData` to guarantee that it is eliminated at compile time.

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

When making your own error type, you need to take a number of steps to make the error type play nicely with the rest of the Rust ecosystem.

1. Your error type should implement the `std::error::Error` trait, which provides callers with common methods for introspecting error types. The main method if interest is `Error::source`, which provides a mechanism to find the underlying cause of an error. This is most commonly used to print a backtrace that displays a trace all the way back to the error's root cause.
2. Your type should implement both `Display` and `Debug` so that callers can meaningfully print your error. This is required if you implement the `Error` trait. In general, your implementation of `Display` should give a one-line description of what went wrong that can easily be folded into other error messages. The display format should be lowercase and without trailing punctuation so that it fits nicely into other, larger error reports. `Debug` should provide a more descriptive error including auxiliary information that may be useful in tracking down the cause of the error, such as port numbers, request identifiers, filepaths, and the like, which `#[derive(Debug)]` is usually sufficient for.
3. Your type should, if possible, implement both `Send` and `Sync` so that users are able to share the error across thread boundaries. If your error type is not thread-safe, you will find that it's almost impossible to use your crate in a multithreaded context. Error types that implement `Send` and `Sync` are also much easier to use with the very common `std::io::Error` type, which is able to wrap errors that implement `Error`, `Send`, and `Sync`.
4. Where possible, your error type should be `'static`.

> In general, the community consensus is that errors should be rare and therefore should not add much cost to the "happy path". For that reason, errors are often placed behind a pointer type, such as a `Box` or `Arc`. This way, they're unlikely to add much to the size of the overall `Result` type they're contained within.

---

> While `Box<dyn Error + ...>` is an attractive type-erased error type, it counterintuitively does not itself implement `Error`. Therefore, consider adding your own `BoxError` type for type erasure in libraries that does implement `Error`.
---
> Keep in mind that `()` does not implement the `Error` trait. This means that it cannot be type-erased into `Box<dyn Error>` and can be a bit of a pain to use with `?`. For this reason, it is often better to define your own unit struct type, implement `Error` for it, and use that as the error instead of `()` in these cases.

### Propagating Errors

Rust's `?` operator acts as a shorthand for _unwrap or return early_, for working easily with errors. But it also has a few other tricks up its sleeve that are worth knowing about.

The `?` operator at the time of writing uses `From`, not `Into`.

1. `?` performs type conversion through the `From` trait. In a function that returns `Result<T, E>`, you can use `?` on any `Result<T, X>` where `E: From<X>`. Under the hood, the `?` operator calls `From::from` on the error value to convert it to a boxed trait object, a `Box<dyn error::Error>`, which is polymorphic-- that means that lots of different kinds of errors can be returned from the same function because all errors act the same since they all implement the `error::Error` trait.
2. `?` operator is really just syntax sugar for a trait tentatively called `Try`

Hint: There are two different possible `Result` types produced within
`main()`, which are propagated using `?` operators. How do we declare a
return type from `main()` that allows both?

Check out this section of the book:
<https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator>

Read more about boxing errors:
<https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/boxing_errors.html>

Read more about using the `?` operator with boxed errors:
<https://doc.rust-lang.org/stable/rust-by-example/error/multiple_error_types/reenter_question_mark.html>
