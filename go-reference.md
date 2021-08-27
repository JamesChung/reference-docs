# Go Reference

## Table of Contents

* [Basic Types](#Basic-Types)
  * [String Types](#String-Types)
  * [Boolean Types](#Boolean-Types)
  * [Integer types](#Integer-Types)
    * [Unsigned Types](#Unsigned-Types)
    * [Signed Types](#Signed-Types)
  * [Floating Point Types](#Floating-Point-Types)
  * [Complex Types](#Complex-Types)
* [Aggregate Types](#Aggregate-Types)
* [Reference Types](#Reference-Types)
* [Type Alias](#Type-Alias)
* [Universe Block](#Universe-Block)
* [Pointer Performance](#Pointer-Performance)
* [Embedding](#Embedding)
  * [Embedding Interfaces](#Embedding-Interfaces)
* [Interfaces](#Interfaces)
  * [Type Assertions](#Type-Assertions)
  * [Type Switches](#Type-Switches)
  * [Function Types](#Function-Types)
* [Errors](#Errors)
  * [Sentinel Errors](#Sentinel-Errors)
  * [Custom Errors](#Custom-Errors)
  * [Wrapping Errors](#Wrapping-Errors)
  * [Recover](#Recover)
* [Modules & Packages](#Modules-&-Packages)
  * [Godoc](#Godoc)
  * [The `internal` Package](#The-internal-Package)
  * [The `init` Function](#The-init-Function)

## Basic Types

### [String Types](https://golang.org/ref/spec#String_types)

* `string`
* `rune`    (`int32`)

### [Boolean Types](https://golang.org/ref/spec#Boolean_types)

* `true`
* `false`

### [Integer Types](https://golang.org/ref/spec#Numeric_types)

#### Unsigned Types

* `uint`    (usually based on CPU arch)
* `uint8`   (`byte`)
* `uint16`
* `uint32`
* `uint64`
* `uintptr` (usually based on CPU arch)

#### Signed Types

* `int`     (usually based on CPU arch)
* `int8`
* `int16`
* `int32`   (`rune`)
* `int64`

### [Floating Point Types](https://golang.org/ref/spec#Numeric_types)

* `float32`
* `float64`

### [Complex Types](https://golang.org/ref/spec#Numeric_types)

* `complex64`
* `complex128`

## Aggregate Types

* [Array](https://golang.org/ref/spec#Array_types) - `[...]array`
* [Struct](https://golang.org/ref/spec#Struct_types) - `struct{}`

## Reference Types

* [Slice](https://golang.org/ref/spec#Slice_types) - `[]type`
* [Channel](https://golang.org/ref/spec#Channel_types) - `chan type`
* [Pointer](https://golang.org/ref/spec#Pointer_types) - `*type`
* [Map](https://golang.org/ref/spec#Map_types) - `map[type]type`
* [Function](https://golang.org/ref/spec#Function_types) - `func(type...)type...`
* [Interface](https://golang.org/ref/spec#Interface_types) - `interface{}`
* Invalid - `reflect.Invalid`

## Type Alias

To create an alias, we use the type keyword, the name of the alias, an equals sign, and the name of the original type. The alias has the same fields and methods as the original type.

The alias can even be assigned to a variable of the original type without a type conversion.

You can alias a type that’s defined in the same package as the original type or in a different package. You can even alias a type from another module.

There is one drawback to an alias in another package: you cannot use an alias to refer to the unexported methods and fields of the original type.

There are two kinds of exported identifiers that can’t have alternate names. The first is a package-level variable. The second is a field in a struct. Once you choose a name for an exported struct field, there’s no way to create an alternate name.

```go
type Foo struct {
    atr string
}

type Bar = Foo

func FooBar(f Foo) {
    // can take both Foo and Bar
}
```

One important point to remember: an alias is just another name for a type. If you want to add new methods or change the fields in an aliased struct, you must add them to the original type.

---

## [Universe Block](https://golang.org/ref/spec#Predeclared_identifiers)

```text
Types:
 bool byte complex64 complex128 error float32 float64
 int int8 int16 int32 int64 rune string
 uint uint8 uint16 uint32 uint64 uintptr

Constants:
 true false iota

Zero value:
 nil

Functions:
 append cap close complex copy delete imag len
 make new panic print println real recover
```

## [Pointer Performance](https://github.com/learning-go-book/pointer_performance)

If a struct is large enough, there are performance improvements from using a pointer to the struct as either an input parameter or a return value. The time to pass a pointer into a function is constant for all data sizes, roughly one nanosecond. This makes sense, as the size of a pointer is the same for all data types. Passing a value into a function takes longer as the data gets larger. It takes about a millisecond once the value gets to be around 10 megabytes of data.

The behavior for returning a pointer versus returning a value is more interesting. For data structures that are smaller than a megabyte, it is actually slower to return a pointer type than a value type. For example, a 100-byte data structure takes around 10 nanoseconds to be returned, but a pointer to that data structure takes about 30 nanoseconds. Once your data structures are larger than a megabyte, the performance advantage flips. It takes nearly 2 milliseconds to return 10 megabytes of data, but a little more than half a millisecond to return a pointer to it.

You should be aware that these are very short times. For the vast majority of cases, the difference between using a pointer and a value won’t affect your program’s performance. But if you are passing megabytes of data between functions, consider using a pointer even if the data is meant to be immutable.

---

To store something on the stack, you have to know exactly how big it is at compile time. When you look at the value types in Go (primitive values, arrays, and structs), they all have one thing in common: we know exactly how much memory they take at compile time. This is why the size is considered part of the type for an array. Because their sizes are known, they can be allocated on the stack instead of the heap. The size of a pointer type is also known, and it is also stored on the stack.

The rules are more complicated when it comes to the data that the pointer points to. In order for Go to allocate the data the pointer points to on the stack, several conditions must be true. It must be a local variable whose data size is known at compile time. The pointer cannot be returned from the function. If the pointer is passed into a function, the compiler must be able to ensure that these conditions still hold. If the size isn’t known, you can’t make space for it by simply moving the stack pointer. If the pointer variable is returned, the memory that the pointer points to will no longer be valid when the function exits. When the compiler determines that the data can’t be stored on the stack, we say that the data the pointer points to escapes the stack and the compiler stores the data on the heap.

The heap is the memory that’s managed by the garbage collector (or by hand in languages like C and C++). Any data that’s stored on the heap is valid as long as it can be tracked back to a pointer type variable on a stack. Once there are no more pointers pointing to that data (or to data that points to that data), the data becomes garbage and it’s the job of the garbage collector to clear it out.

A common source of bugs in C programs is returning a pointer to a local variable. In C, this results in a pointer pointing to invalid memory. The Go compiler is smarter. When it sees that a pointer to a local variable is returned, the local variable’s value is stored on the heap.

## Embedding

While embedding one concrete type inside another won’t allow you to treat the outer type as the inner type, the methods on an embedded field do count toward the method set of the containing struct. This means they can make the containing struct implement an interface.

```go
type Metadata struct {
  // ...
}

type BaseError struct {
  msg      string
  metadata Metadata
}

func (be *BaseError) Error() string {
  return fmt.Sprintf("%s\n%#v", be.msg, be.metadata)
}

// HTTPError implicitly implements error interface via embedding BaseError
type HTTPError struct {
  BaseError
  code int
}

func NewHTTPError(msg, metadata Metadata, code int) *HTTPError {
  return &HTTPError{
    BaseError: &BaseError{
      msg:      msg,
      metadata: metadata,
    },
    code: code,
  }
}
```

### Embedding Interfaces

Just like you can embed a type in a struct, you can also embed an interface in an interface via an anonymous field. For example, the io.ReadCloser interface is built out of an io.Reader and an io.Closer:

```go
type Reader interface {
  Read(p[]byte) (n int, err error)
}

type Closer interface {
  Close() error
}

type ReadCloser interface {
  Reader
  Closer
}
```

Just like you can embed a concrete type in a struct, you can also embed an interface in a struct via an anonymous field.

```go
package main

import (
  "context"
  "errors"
  "fmt"
)

type BaseError struct {
  error
}

func main() {
  var err error
  // Type *BaseError which embeds error which allows this struct to implement error interface
  baseError = &BaseError{
    error: errors.New("error"),
  }
  // err can be assigned baseError because *BaseError implicitly implements the error interface
  err = baseError
  // because error is embeded, the Error() method can be implicitly used
  baseError.Error()
  // explicitly invoked via the error field instead
  baseError.error.baseError()
  // err interface type uses underlying concrete *BaseError's Error() method
  err.Error()
}
```

## Interfaces

* If you need a data structure beyond a slice, array, or map, and you don’t want it to only work with a single type, you need to use a field of type `interface{}` to hold its value.
* An empty interface type simply states that the variable can store any value whose type implements zero or more methods.
* Rather than writing a single factory function that returns different instances behind an interface based on input parameters, try to write separate factory functions for each concrete type.
* when invoking a function with parameters of interface types, a heap allocation occurs for each of the interface parameters.
* In order for an interface to be considered nil both the type and the value must be nil.
* In the Go runtime, interfaces are implemented as a pair of pointers, one to the underlying type and one to the underlying value. As long as the type is non-nil, the interface is non-nil. (Since you cannot have a variable without a type, if the value pointer is non-nil, the type pointer is always non-nil.)
* If an interface is nil, invoking any methods on it triggers a panic

### Type Assertions

#### The comma ok Idiom:

```go
val, ok := i.(int)
if !ok {
  return fmt.Errorf("unexpected type for %v", i)
}
// continue logic...
```

The boolean ok is set to true if the type conversion was successful.

If it was not, ok is set to false and the other variable is set to its zero value.

### Type Switches

A type switch looks a lot like the switch statement, but instead of specifying a boolean operation, you specify a variable of an interface type and follow it with `.(type)`.

_**Note:**_
If you list more than one type on a case, the new variable is of type `interface{}`.

```go
switch val := i.(type) {
case nil:
  // i is nil, type of val is interface{}
case int:
  // val is of type int
case string:
  // val is of type string
default:
  // no idea what i is, val is defaulted to type interface{}
}
```

**Note:**

Since the purpose of a type switch is to derive a new variable from an existing one, it is idiomatic to assign the variable being switched on to a variable of the same name `(i := i.(type))`, making this one of the few places where shadowing is a good idea. To make the comments more readable, our example doesn’t use shadowing.

### Function Types

_**Go allows methods on any user-defined type, including user-defined function types.**_

They allow functions to implement interfaces. The most common usage is for HTTP handlers.

By using a type conversion to http.HandlerFunc, any function that has the signature `func(http.ResponseWriter,*http.Request)` can be used as an http.Handler:

```go
type HandlerFunc func(http.ResponseWriter, *http.Request)

func (f HandlerFunc) ServeHTTP(writer http.ResponseWriter, request *http.Request) {
  f(writer, reader)
}
```

This lets you implement HTTP handlers using functions, methods, or closures using the exact same code path as the one used for other types that meet the `http.Handler` interface.

The question becomes: when should your function or method specify an input parameter of a function type and when should you use an interface?

If your single function is likely to depend on many other functions or other state that’s not specified in its input parameters, use an interface parameter and define a function type to bridge a function to the interface.

## Errors

Go handles errors by returning a value of type error as the last return value for a function. This is entirely by convention, but it is such a strong convention that it should never be breached.

When a function executes as expected, nil is returned for the error parameter. If something goes wrong, an error value is returned instead.

Error messages should not be capitalized nor should they end with punctuation or a newline.

In most cases, you should set the other return values to their zero values when a non-nil error is returned.

`error` is a built-in interface that defines a single method:

```go
type error interface {
  Error() string
}
```

Anything that implements this interface is considered an error.

### Sentinel Errors

_The name descends from the practice in computer programming of using a specific value to signify that no further processing is possible. So to with Go, we use specific values to signify an error._

* Sentinel errors are one of the few variables that are declared at the package level.

* By convention, their names start with Err (with the notable exception of io.EOF).

* They should be treated as read-only; there’s no way for the Go compiler to enforce this, but it is a programming error to change their value.
Sentinel errors are usually used to indicate that you cannot start or continue processing.

* Be sure you need a sentinel error before you define one. Once you define one, it is part of your public API and you have committed to it being available in all future backward-compatible releases.

* It’s far better to reuse one of the existing ones in the standard library or to define an error type that includes information about the condition that caused the error to be returned

```go
func main() {
  data := []byte("This is not a zip file")
  notAZipFile := bytes.NewReader(data)
  _, err := zip.NewReader(notAZipFile, int64(len(data)))
  if err == zip.ErrFormat {
    fmt.Println("Told you so")
  }
}
```

If you have an error condition that indicates a specific state has been reached in your application where no further processing is possible and no contextual information needs to be used to explain the error state, a sentinel error is the correct choice.

### Custom Errors

When using custom errors, never define a variable to be of the type of your custom error. Either explicitly return nil when no error occurs or define the variable to be of type error.

**WARNING: Don’t use a type assertion or a type switch to access the fields and methods of a custom error. Instead, use `errors.As`**

### Wrapping Errors

When an error is passed back through your code, you often want to add additional context to it. This context can be the name of the function that received the error or the operation it was trying to perform. When you preserve an error while adding additional information, it is called wrapping the error. When you have a series of wrapped errors, it is called an error chain.

There’s a function in the Go standard library that wraps errors, and we’ve already seen it. The `fmt.Errorf` function has a special verb, %w. Use this to create an error whose formatted string includes the formatted string of another error and which contains the original error as well. The convention is to write `: %w` at the end of the error format string and make the error to be wrapped the last parameter passed to `fmt.Errorf`.

#### Unwrap

The standard library also provides a function for unwrapping errors, the `Unwrap` function in the errors package. You pass it an error and it returns the wrapped error, if there is one. If there isn’t, it returns `nil`.

```go
func fileChecker(name string) error {
    f, err := os.Open(name)
    if err != nil {
        return fmt.Errorf("in fileChecker: %w", err)
    }
    f.Close()
    return nil
}

func main() {
    err := fileChecker("not_here.txt")
    if err != nil {
        fmt.Println(err)
        if wrappedErr := errors.Unwrap(err); wrappedErr != nil {
            fmt.Println(wrappedErr)
        }
    }
}
```

You don’t usually call `errors.Unwrap` directly. Instead, you use `errors.Is` and `errors.As` to find a specific wrapped error.

**Note: If you want to wrap an error with your custom error type, your error type needs to implement the method Unwrap. This method takes in no parameters and returns an error.**

```go
type StatusErr struct {
    Status Status
    Message string
    err error
}

func (se StatusErr) Error() string {
    return se.Message
}

func (se StatusError) Unwrap() error {
    return se.err
}

// Wrap underlying errors with StatusErr
func LoginAndGetData(uid, pwd, file string) ([]byte, error) {
    err := login(uid,pwd)
    if err != nil {
        return nil, StatusErr {
            Status: InvalidLogin,
            Message: fmt.Sprintf("invalid credentials for user %s",uid),
            Err: err,
        }
    }
    data, err := getData(file)
    if err != nil {
        return nil, StatusErr {
            Status: NotFound,
            Message: fmt.Sprintf("file %s not found",file),
            Err: err,
        }
    }
    return data, nil
}
```

If you want to create a new error that contains the message from another error, but don’t want to wrap it, use `fmt.Errorf` to create an error, but use the `%v` verb instead of `%w`:

```go
err := internalFunction()
if err != nil {
    return fmt.Errorf("internal failure: %v", err)
}
```

#### Is and As

Use `errors.Is` when you are looking for a specific instance or specific values. Use `errors.As` when you are looking for a specific type.

##### Is

The `errors.Is` function returns `true` if there is an error in the error chain that matches the provided sentinel error.

By default, `errors.Is` uses `==` to compare each wrapped error with the specified error. If this does not work for an error type that you define (for example, if your error is a noncomparable type), implement the `Is` method on your error:

```go
type MyErr struct {
    Codes []int
}

func (me MyErr) Error() string {
    return fmt.Sprintf("codes: %v", me.Codes)
}

func (me MyErr) Is(target error) bool {
    if me2, ok := target.(MyErr); ok {
        return reflect.DeepEqual(me, me2)
    }
    return false
}
```

##### As

The `errors.As` function allows you to check if a returned error (or any error it wraps) matches a specific type.

You don’t have to pass a pointer to a variable of an error type as the second parameter to `errors.As`. You can pass a pointer to an interface to find an error that meets the interface.

```go
err := AFunctionThatReturnsAnError()
var coder interface {
    Code() int
}
if errors.As(err, &coder) {
    fmt.Println(coder.Code())
}
```

### Recover

As soon as a panic happens, the current function exits immediately and any defers attached to the current function start running.

When those defers complete, the defers attached to the calling function run, and so on, until main is reached. The program then exits with a message and a stack trace.

The built-in recover function is called from within a defer to check if a panic happened. If there was a panic, the value assigned to the panic is returned. Once a recover happens, execution continues normally.

```go
func div60(i int) {
    defer func() {
        if v := recover(); v != nil {
            fmt.Println(v)
        }
    }()
    fmt.Println(60 / i)
}
```

**There’s a specific pattern for using recover. We register a function with defer to handle a potential panic. We call recover within an if statement and check to see if a non-nil value was found. You must call recover from within a defer because once a panic happens, only deferred functions are run.**

**There is one situation where recover is recommended.**
If you are creating a library for third parties, do not let panics escape the boundaries of your public API. If a panic is possible, a public function should use a recover to convert the panic into an error, return it, and let the calling code decide what to do with them.

## Modules & Packages

* The package name . places all the exported identifiers in the imported package into the current package’s namespace; you don’t need a prefix to refer to them.

* Package names can be shadowed.

### Godoc

The rules/conventions are:

* Place the comment directly before the item being documented with no blank lines between the comment and the declaration of the item.

* Start the comment with two forward slashes (//) followed by the name of the item.

* Use a blank comment to break your comment into multiple paragraphs.

* Insert preformatted comments by indenting the lines.

Comments before the package declaration create package-level comments. If you have lengthy comments for the package (such as the extensive formatting documentation in the fmt package), the convention is to put the comments in a file in your package called `doc.go`.

### The `internal` Package

Sometimes you want to share a function, type, or constant between packages in your module, but you don’t want to make it part of your API. Go supports this via the special internal package name.

When you create a package called internal, the exported identifiers in that package and its subpackages are only accessible to the direct parent package of internal and the sibling packages of internal.

### The `init` Function

When you declare a function named init that takes no parameters and returns no values, it runs the first time the package is referenced by another package.

Since init functions do not have any inputs or outputs, they can only work by side effect, interacting with package-level functions and variables.

Go allows you to declare multiple init functions in a single package, or even in a single file in a package.

The primary use of init functions today is to initialize package-level variables that can’t be configured in a single assignment.

You should only declare a single init function per package, even though Go allows you to define multiple.
