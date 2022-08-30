# Go Reference

# Table of contents

- [Go Reference](#go-reference)
  - [Table of contents](#table-of-contents)
  - [Commands](#commands)
  - [Tuple assignments](#tuple-assignments)
  - [Do While Loops](#do-while-loops)
  - [iota](#iota)
  - [Package Initialization](#package-initialization)
  - [Basic Types](#basic-types)
    - [String Types](#string-types)
      - [Formatting](#formatting)
    - [Boolean Types](#boolean-types)
    - [Integer Types](#integer-types)
    - [Floating Point Types](#floating-point-types)
    - [Complex Types](#complex-types)
  - [Aggregate Types](#aggregate-types)
  - [Reference Types](#reference-types)
  - [Type Alias](#type-alias)
  - [Universe Block](#universe-block)
  - [Pointer Performance](#pointer-performance)
  - [Embedding](#embedding)
    - [Embedding Interfaces](#embedding-interfaces)
  - [Methods](#methods)
    - [Method Values and Expressions](#method-values-and-expressions)
  - [Interfaces](#interfaces)
    - [Type Assertions](#type-assertions)
      - [The comma ok Idiom](#the-comma-ok-idiom)
    - [Type Switches](#type-switches)
    - [Type Assertions](#type-assertions)
    - [Function Types](#function-types)
  - [Errors](#errors)
    - [Sentinel Errors](#sentinel-errors)
    - [Custom Errors](#custom-errors)
    - [Wrapping Errors](#wrapping-errors)
      - [Unwrap](#unwrap)
      - [Is and As](#is-and-as)
        - [Is](#is)
        - [As](#as)
    - [Recover](#recover)
  - [Modules and Packages](#modules-and-packages)
    - [Godoc](#godoc)
    - [The `internal` Package](#the-internal-package)
    - [The `init` Function](#the-init-function)
    - [Versioning](#versioning)

## Commands

`go list -m -versions <module path>`

`go install <module path>@<version || tag>`

`go get -u=patch <module path>`

## Tuple assignments

```go
v, ok := m[key] // map lookup
v, ok := x.(T)  // type assertion
v, ok := <-ch   // channel receive
```

## Do While Loops

```go
for {
    // things to do in the loop
    if !CONDITION {
        break
    }
}
```

## iota

_Don’t use iota for defining constants where its values are explicitly defined
(elsewhere). For example, when implementing parts of a specification and the
specification says which values are assigned to which constants, you should
explicitly write the constant values. Use iota for “internal” purposes only.
That is, where the constants are referred to by name rather than by value. That
way you can optimally enjoy iota by inserting new constants at any moment in
time / location in the list without the risk of breaking everything._

## Package Initialization

1. Dependencies
2. Package level variables, if the package has multiple `.go` files, they are
   initialized in the order in which the files are given to the compiler; the
   `go` tool sorts `.go` files by name before invoking the compiler
3. Each variable declared at package level starts life with the value of its
   initializer expression if any.
4. `init()` function - Any file can contain any number of `init` functions, they
   are executed in the order in which they are declared.
5. One package is initialized at a time, in the order of imports in the program,
   dependencies first.
6. `main` package is the last to be initialized. All packages are fully
   initialized before the application's `main` function begins.

## Basic Types

### String Types

> <https://golang.org/ref/spec#String_types>

- `string`
- `rune` (`int32`)

> A `[]rune` conversion applied to a UTF-8 encoded string returns the sequence
> of Unicode code points that the string encodes.

#### Formatting

Variable indexing

```go
o := 0666
fmt.Printf("%d %[1]o %#[1]o\n", o) // 438 666 0666
x := int64(0xdeadbeef)
fmt.Printf("%d %[1]x %#[1]x %#[1]X\n", x)
// Output:
// 3735928559 deadbeef 0xdeadbeef 0XDEADBEEF
```

### Boolean Types

> <https://golang.org/ref/spec#Boolean_types>

- `true`
- `false`

### Integer Types

> <https://golang.org/ref/spec#Numeric_types>

- Unsigned Types
  - `uint` (usually based on CPU arch)
  - `uint8` (`byte`)
  - `uint16`
  - `uint32`
  - `uint64`
  - `uintptr` (usually based on CPU arch)
- Signed Types
  - `int` (usually based on CPU arch)
  - `int8`
  - `int16`
  - `int32` (`rune`)
  - `int64`

> Regardless of their size, `int`, `uint`, and `uintptr` are different types
> from their explicitly sized siblings. Thus `int` is not the same type as
> `int32`, even if the natural size of integers is 32 bits, and an explicit
> conversion is required to sue an `int` value where an `int32` is needed, and
> vice versa.

### Floating Point Types

> <https://golang.org/ref/spec#Numeric_types>

- `float32`
- `float64`

### Complex Types

> <https://golang.org/ref/spec#Numeric_types>

- `complex64`
- `complex128`

## Aggregate Types

- [Array](https://golang.org/ref/spec#Array_types) - `[...]array`
- [Struct](https://golang.org/ref/spec#Struct_types) - `struct{}`

## Reference Types

- [Slice](https://golang.org/ref/spec#Slice_types) - `[]type`
- [Channel](https://golang.org/ref/spec#Channel_types) - `chan type`
- [Pointer](https://golang.org/ref/spec#Pointer_types) - `*type`
- [Map](https://golang.org/ref/spec#Map_types) - `map[type]type`
- [Function](https://golang.org/ref/spec#Function_types) -
  `func(type...)type...`
- [Interface](https://golang.org/ref/spec#Interface_types) - `interface{}`
- Invalid - `reflect.Invalid`

## Type Alias

To create an alias, we use the type keyword, the name of the alias, an equals
sign, and the name of the original type. The alias has the same fields and
methods as the original type.

The alias can even be assigned to a variable of the original type without a type
conversion.

You can alias a type that’s defined in the same package as the original type or
in a different package. You can even alias a type from another module.

There is one drawback to an alias in another package: you cannot use an alias to
refer to the unexported methods and fields of the original type.

There are two kinds of exported identifiers that can’t have alternate names. The
first is a package-level variable. The second is a field in a struct. Once you
choose a name for an exported struct field, there’s no way to create an
alternate name.

```go
type Foo struct {
    atr string
}

type Bar = Foo

func FooBar(f Foo) {
    // can take both Foo and Bar
}
```

One important point to remember: an alias is just another name for a type. If
you want to add new methods or change the fields in an aliased struct, you must
add them to the original type.

---

## Universe Block

> <https://golang.org/ref/spec#Predeclared_identifiers>

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

## Pointer Performance

> <https://github.com/learning-go-book/pointer_performance>

If a struct is large enough, there are performance improvements from using a
pointer to the struct as either an input parameter or a return value. The time
to pass a pointer into a function is constant for all data sizes, roughly one
nanosecond. This makes sense, as the size of a pointer is the same for all data
types. Passing a value into a function takes longer as the data gets larger. It
takes about a millisecond once the value gets to be around 10 megabytes of data.

The behavior for returning a pointer versus returning a value is more
interesting. For data structures that are smaller than a megabyte, it is
actually slower to return a pointer type than a value type. For example, a
100-byte data structure takes around 10 nanoseconds to be returned, but a
pointer to that data structure takes about 30 nanoseconds. Once your data
structures are larger than a megabyte, the performance advantage flips. It takes
nearly 2 milliseconds to return 10 megabytes of data, but a little more than
half a millisecond to return a pointer to it.

You should be aware that these are very short times. For the vast majority of
cases, the difference between using a pointer and a value won’t affect your
program’s performance. But if you are passing megabytes of data between
functions, consider using a pointer even if the data is meant to be immutable.

---

To store something on the stack, you have to know exactly how big it is at
compile time. When you look at the value types in Go (primitive values, arrays,
and structs), they all have one thing in common: we know exactly how much memory
they take at compile time. This is why the size is considered part of the type
for an array. Because their sizes are known, they can be allocated on the stack
instead of the heap. The size of a pointer type is also known, and it is also
stored on the stack.

The rules are more complicated when it comes to the data that the pointer points
to. **In order for Go to allocate the data the pointer points to on the stack,
several conditions must be true. It must be a local variable whose data size is
known at compile time. The pointer cannot be returned from the function. If the
pointer is passed into a function, the compiler must be able to ensure that
these conditions still hold. If the size isn’t known, you can’t make space for
it by simply moving the stack pointer. If the pointer variable is returned, the
memory that the pointer points to will no longer be valid when the function
exits. When the compiler determines that the data can’t be stored on the stack,
we say that the data the pointer points to escapes the stack and the compiler
stores the data on the heap.**

The heap is the memory that’s managed by the garbage collector (or by hand in
languages like C and C++). Any data that’s stored on the heap is valid as long
as it can be tracked back to a pointer type variable on a stack. Once there are
no more pointers pointing to that data (or to data that points to that data),
the data becomes garbage and it’s the job of the garbage collector to clear it
out.

> A common source of bugs in C programs is returning a pointer to a local
> variable. In C, this results in a pointer pointing to invalid memory. The Go
> compiler is smarter. When it sees that a pointer to a local variable is
> returned, the local variable’s value is stored on the heap.

The escape analysis done by the Go compiler isn’t perfect. There are some cases
where data that could be stored on the stack escapes to the heap. However, the
compiler has to be conservative; it can’t take the chance of leaving a value on
the stack when it might need to be on the heap because leaving a reference to
invalid data causes memory corruption.

RAM might mean “random access memory,” but the fastest way to read from memory
is to read it sequentially. A slice of structs in Go has all of the data laid
out sequentially in memory. This makes it fast to load and fast to process. A
slice of pointers to structs (or structs whose fields are pointers) has its data
scattered across RAM, making it far slower to read and process.

## Embedding

While embedding one concrete type inside another won’t allow you to treat the
outer type as the inner type, the methods on an embedded field do count toward
the method set of the containing struct. This means they can make the containing
struct implement an interface.

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

> Methods can be declared only on named types and pointers to them, but thanks
> to embedding, it's possible and sometimes useful for _unnamed_ struct types to
> have methods too. Because the `sync.Mutex` field is embedded within it, its
> `Lock` and `Unlock` methods are promoted to the unnamed struct type, allowing
> us to lock the `cache` with a self-explanatory syntax.

```go
var cache = struct {
  sync.Mutex
  mapping map[string]string
} {
  mapping: make(map[string]string),
}

func Lookup(key string) string {
  cache.Lock()
  v := cache.mapping[key]
  cache.Unlock()
  return v
}
```

### Embedding Interfaces

Just like you can embed a type in a struct, you can also embed an interface in
an interface via an anonymous field. For example, the io.ReadCloser interface is
built out of an io.Reader and an io.Closer:

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

Just like you can embed a concrete type in a struct, you can also embed an
interface in a struct via an anonymous field.

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
  // because error is embedded, the Error() method can be implicitly used
  baseError.Error()
  // explicitly invoked via the error field instead
  baseError.error.baseError()
  // err interface type uses underlying concrete *BaseError's Error() method
  err.Error()
}
```

## Methods

### Method Values and Expressions

Usually we select and call a method in the same expression, as in
`p.Distance()`, but it's possible to separate these two operations. The selector
`p.Distance` yields a _method value_, a function that binds a method
(`Point.Distance`) to a specific receiver value `p`. This function can then be
invoked without a receiver value; it needs only the non-receiver arguments.

```go
p := Point{1, 2}
q := Point{4, 6}

distanceFromP := p.Distance   // Method value
fmt.Println(distanceFromP(q)) // "5"

scaleP := p.ScaleBy           // Method value
scaleP(2)                     // p becomes (2, 4)
scaleP(3)                     //      then (6, 12)
scaleP(10)                    //      then (60, 120)
```

Related to the method value is the _method expression_. When calling a method,
as opposed to an ordinary function, we must supply the receiver in a special way
using the selector syntax. A method expression, written `T.f` or `(*T).f` where
`T` is a type, yields a function value with a regular first parameter taking the
place of the receiver, so it can be called in the usual way.

```go
p := Point{1, 2}
q := Point{4, 6}

distance := Point.Distance    // method expression
fmt.Println(distance(p, q))   // "5"
fmt.Printf("%T\n", distance)  // func(Point, Point) float64

scale := (*Point).ScaleBy
scale(&p, 2)
fmt.Println(p)                // "{2, 4}"
fmt.Printf("%T\n", scale)     // func(*Point, float64)
```

## Interfaces

- If you need a data structure beyond a slice, array, or map, and you don’t want
  it to only work with a single type, you need to use a field of type
  `interface{}` to hold its value.
- An empty interface type simply states that the variable can store any value
  whose type implements zero or more methods.
- Rather than writing a single factory function that returns different instances
  behind an interface based on input parameters, try to write separate factory
  functions for each concrete type.
- When invoking a function with parameters of interface types, a heap allocation
  occurs for each of the interface parameters.
  - Figuring out the trade-off between better abstraction and better performance
    is something that should be done over the life of your program. Write your
    code so that it is readable and maintainable. If you find that your program
    is too slow and you have profiled it and you have determined that the
    performance problems are due to a heap allocation caused by an interface
    parameter, then you should rewrite the function to use a concrete type
    parameter.
- In order for an interface to be considered nil both the type and the value
  must be nil.
- In the Go runtime, interfaces are implemented as a pair of pointers, one to
  the underlying type and one to the underlying value. As long as the type is
  non-nil, the interface is non-nil. (Since you cannot have a variable without a
  type, if the value pointer is non-nil, the type pointer is always non-nil.)
  - What nil indicates for an interface is whether or not you can invoke methods
    on it.
- If an interface is nil, invoking any methods on it triggers a panic

### Type Assertions

#### The comma ok Idiom

```go
val, ok := i.(int)
if !ok {
  return fmt.Errorf("unexpected type for %v", i)
}
// continue logic...
```

The boolean ok is set to true if the type conversion was successful.

If it was not, ok is set to false and the other variable is set to its zero
value.

### Type Switches

A type switch looks a lot like the switch statement, but instead of specifying a
boolean operation, you specify a variable of an interface type and follow it
with `.(type)`.

_**Note:**_ If you list more than one type on a case, the new variable is of
type `interface{}`.

```go
switch j := i.(type) {
    case nil:
        // i is nil, type of j is interface{}
    case int:
        // j is of type int
    case MyInt:
        // j is of type MyInt
    case io.Reader:
        // j is of type io.Reader
    case string:
        // j is a string
    case bool, rune:
        // i is either a bool or rune, so j is of type interface{}
    default:
        // no idea what i is, so j is of type interface{}
}
```

**Note:**

Since the purpose of a type switch is to derive a new variable from an existing
one, it is idiomatic to assign the variable being switched on to a variable of
the same name `(i := i.(type))`, making this one of the few places where
shadowing is a good idea. To make the comments more readable, our example
doesn’t use shadowing.

The type of the new variable depends on which case matches. You can use `nil`
for one case to see if the interface has no associated type. If you list more
than one type on a case, the new variable is of type `interface{}`.

While it might seem handy to be able to extract the concrete implementation from
an interface variable, you should use these techniques infrequently. For the
most part, treat a parameter or return value as the type that was supplied and
not what else it could be. Otherwise, your function’s API isn’t accurately
declaring what types it needs to perform its task. If you needed a different
type, then it should be specified.

### Type Assertions

One common use of a type assertion is to see if the concrete type behind the
interface also implements another interface.

```go
// copyBuffer is the actual implementation of Copy and CopyBuffer.
// if buf is nil, one is allocated.
func copyBuffer(dst Writer, src Reader, buf []byte) (written int64, err error) {
    // If the reader has a WriteTo method, use it to do the copy.
    // Avoids an allocation and a copy.
    if wt, ok := src.(WriterTo); ok {
        return wt.WriteTo(dst)
    }
    // Similarly, if the writer has a ReadFrom method, use it to do the copy.
    if rt, ok := dst.(ReaderFrom); ok {
        return rt.ReadFrom(src)
    }
    // function continues...
}
```

---

```go
package main

import (
	"fmt"
)

type Dog interface {
	Bark() string
}

type Person struct {
	Name string
}

func (p Person) String() string {
	return p.Name
}

func (p Person) Bark() string {
	return "Bark!"
}

func main() {
	fmt.Println("Start...")
	var blah fmt.Stringer
	blah = Person{
		Name: "Jack",
	}
	if t, ok := blah.(Dog); ok {
		fmt.Println("blah also implements Animal", t)
	}
	fmt.Println("Done...")
}
```

### Function Types

_**Go allows methods on any user-defined type, including user-defined function
types.**_

They allow functions to implement interfaces. The most common usage is for HTTP
handlers.

By using a type conversion to http.HandlerFunc, any function that has the
signature `func(http.ResponseWriter,*http.Request)` can be used as an
http.Handler:

```go
type HandlerFunc func(http.ResponseWriter, *http.Request)

func (f HandlerFunc) ServeHTTP(writer http.ResponseWriter, request *http.Request) {
  f(writer, reader)
}
```

This lets you implement HTTP handlers using functions, methods, or closures
using the exact same code path as the one used for other types that meet the
`http.Handler` interface.

The question becomes: when should your function or method specify an input
parameter of a function type and when should you use an interface?

If your single function is likely to depend on many other functions or other
state that’s not specified in its input parameters, use an interface parameter
and define a function type to bridge a function to the interface.

## Errors

Go handles errors by returning a value of type error as the last return value
for a function. This is entirely by convention, but it is such a strong
convention that it should never be breached.

When a function executes as expected, nil is returned for the error parameter.
If something goes wrong, an error value is returned instead.

Error messages should not be capitalized nor should they end with punctuation or
a newline.

In most cases, you should set the other return values to their zero values when
a non-nil error is returned.

`error` is a built-in interface that defines a single method:

```go
type error interface {
  Error() string
}
```

Anything that implements this interface is considered an error.

### Sentinel Errors

_The name descends from the practice in computer programming of using a specific
value to signify that no further processing is possible. So to with Go, we use
specific values to signify an error._

- Sentinel errors are one of the few variables that are declared at the package
  level.

- By convention, their names start with Err (with the notable exception of
  io.EOF).

- They should be treated as read-only; there’s no way for the Go compiler to
  enforce this, but it is a programming error to change their value. Sentinel
  errors are usually used to indicate that you cannot start or continue
  processing.

- Be sure you need a sentinel error before you define one. Once you define one,
  it is part of your public API and you have committed to it being available in
  all future backward-compatible releases.

- It’s far better to reuse one of the existing ones in the standard library or
  to define an error type that includes information about the condition that
  caused the error to be returned

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

If you have an error condition that indicates a specific state has been reached
in your application where no further processing is possible and no contextual
information needs to be used to explain the error state, a sentinel error is the
correct choice.

### Custom Errors

When using custom errors, never define a variable to be of the type of your
custom error. Either explicitly return nil when no error occurs or define the
variable to be of type error.

**WARNING: Don’t use a type assertion or a type switch to access the fields and
methods of a custom error. Instead, use `errors.As`**

### Wrapping Errors

When an error is passed back through your code, you often want to add additional
context to it. This context can be the name of the function that received the
error or the operation it was trying to perform. When you preserve an error
while adding additional information, it is called wrapping the error. When you
have a series of wrapped errors, it is called an error chain.

There’s a function in the Go standard library that wraps errors, and we’ve
already seen it. The `fmt.Errorf` function has a special verb, %w. Use this to
create an error whose formatted string includes the formatted string of another
error and which contains the original error as well. The convention is to write
`: %w` at the end of the error format string and make the error to be wrapped
the last parameter passed to `fmt.Errorf`.

#### Unwrap

The standard library also provides a function for unwrapping errors, the
`Unwrap` function in the errors package. You pass it an error and it returns the
wrapped error, if there is one. If there isn’t, it returns `nil`.

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

You don’t usually call `errors.Unwrap` directly. Instead, you use `errors.Is`
and `errors.As` to find a specific wrapped error.

**Note: If you want to wrap an error with your custom error type, your error
type needs to implement the method Unwrap. This method takes in no parameters
and returns an error.**

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

If you want to create a new error that contains the message from another error,
but don’t want to wrap it, use `fmt.Errorf` to create an error, but use the `%v`
verb instead of `%w`:

```go
err := internalFunction()
if err != nil {
    return fmt.Errorf("internal failure: %v", err)
}
```

#### Is and As

Use `errors.Is` when you are looking for a specific instance or specific values.
Use `errors.As` when you are looking for a specific type.

##### Is

The `errors.Is` function returns `true` if there is an error in the error chain
that matches the provided sentinel error.

By default, `errors.Is` uses `==` to compare each wrapped error with the
specified error. If this does not work for an error type that you define (for
example, if your error is a noncomparable type), implement the `Is` method on
your error:

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

The `errors.As` function allows you to check if a returned error (or any error
it wraps) matches a specific type.

You don’t have to pass a pointer to a variable of an error type as the second
parameter to `errors.As`. You can pass a pointer to an interface to find an
error that meets the interface.

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

As soon as a panic happens, the current function exits immediately and any
defers attached to the current function start running.

When those defers complete, the defers attached to the calling function run, and
so on, until main is reached. The program then exits with a message and a stack
trace.

The built-in recover function is called from within a defer to check if a panic
happened. If there was a panic, the value assigned to the panic is returned.
Once a recover happens, execution continues normally.

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

**There’s a specific pattern for using recover. We register a function with
defer to handle a potential panic. We call recover within an if statement and
check to see if a non-nil value was found. You must call recover from within a
defer because once a panic happens, only deferred functions are run.**

**There is one situation where recover is recommended.** If you are creating a
library for third parties, do not let panics escape the boundaries of your
public API. If a panic is possible, a public function should use a recover to
convert the panic into an error, return it, and let the calling code decide what
to do with them.

## Modules and Packages

- The package name . places all the exported identifiers in the imported package
  into the current package’s namespace; you don’t need a prefix to refer to
  them.

- Package names can be shadowed.

Whenever you run any go command that requires dependencies (such as go run, go
build, go test, or even go list), any imports that aren’t already in go.mod are
downloaded to a cache.

- The go.mod file is automatically updated to include the module path that
  contains the package and the version of the module.

- The go.sum file is updated with two entries: one with the module, its version,
  and a hash of the module, the other with the hash of the go.mod file for the
  module.

### Godoc

The rules/conventions are:

- Place the comment directly before the item being documented with no blank
  lines between the comment and the declaration of the item.

- Start the comment with two forward slashes (//) followed by the name of the
  item.

- Use a blank comment to break your comment into multiple paragraphs.

- Insert preformatted comments by indenting the lines.

Comments before the package declaration create package-level comments. If you
have lengthy comments for the package (such as the extensive formatting
documentation in the fmt package), the convention is to put the comments in a
file in your package called `doc.go`.

### The `internal` Package

Sometimes you want to share a function, type, or constant between packages in
your module, but you don’t want to make it part of your API. Go supports this
via the special internal package name.

When you create a package called internal, the exported identifiers in that
package and its subpackages are only accessible to the direct parent package of
internal and the sibling packages of internal.

### The `init` Function

When you declare a function named init that takes no parameters and returns no
values, it runs the first time the package is referenced by another package.

Since init functions do not have any inputs or outputs, they can only work by
side effect, interacting with package-level functions and variables.

Go allows you to declare multiple init functions in a single package, or even in
a single file in a package.

The primary use of init functions today is to initialize package-level variables
that can’t be configured in a single assignment.

You should only declare a single init function per package, even though Go
allows you to define multiple.

### Versioning

Go supports two ways for creating the different import paths:

- Create a subdirectory within your module named _vN_, where _N_ is the major
  version of your module. For example, if you are creating version 2 of your
  module, call this directory _v2_. Copy your code into this subdirectory,
  including the _README_ and _LICENSE_ files.

- Create a branch in your version control system. You can either put the old
  code on the branch or the new code. Name the branch _vN_ if you are putting
  the new code on the branch, or _vN-1_ if you are putting the old code there.
  For example, if you are creating version 2 of your module and want to put
  version 1 code on the branch, name the branch _v1_.

When upgrading a major version you can use
[mod](https://github.com/marwan-at-work/mod) to automate the renaming process of
packages.

### Goroutines

1. A process is an instance of a program that’s being run by a computer’s operating system.
2. The operating system associates some resources, such as memory, with the process and makes sure that other processes can’t access them.
3. A process is composed of one or more threads.
4. A thread is a unit of execution that is given some time to run by the operating system.
5. Threads within a process share access to resources.
6. A CPU can execute instructions from one or more threads at the same time, depending on the number of cores.
7. One of the jobs of an operating system is to schedule threads on the CPU to make sure that every process (and every thread within a process) gets a chance to run.

Goroutines are lightweight processes managed by the Go runtime. When a Go program starts, the Go runtime creates a number of threads and launches a single goroutine to run your program. All of the goroutines created by your program, including the initial one, are assigned to these threads automatically by the Go runtime scheduler, just as the operating system schedules threads across CPU cores. This might seem like extra work, since the underlying operating system already includes a scheduler that manages threads and processes, but it has several benefits:

- Goroutine creation is faster than thread creation, because you aren’t creating an operating system–level resource.
- Goroutine initial stack sizes are smaller than thread stack sizes and can grow as needed. This makes goroutines more memory efficient.
- Switching between goroutines is faster than switching between threads because it happens entirely within the process, avoiding operating system calls that are (relatively) slow.
- The scheduler is able to optimize its decisions because it is part of the Go process. The scheduler works with the network poller, detecting when a goroutine can be unscheduled because it is blocking on I/O. It also integrates with the garbage collector, making sure that work is properly balanced across all of the operating system threads assigned to your Go process.

### Channels

Goroutines communicate using _channels_. Like slices and maps, channels are a built-in type created using the `make` function:

```go
ch := make(chan int)
```

Like maps, channels are reference types. When you pass a channel to a function, you are really passing a pointer to the channel. Also like maps and slices, the zero value for a channel is `nil`.

#### Reading, Writing, and Buffering

Use the `<-` operator to interact with a channel. You read from a channel by placing the `<-` operator to the left of the channel variable, and you write to a channel by placing it to the right:

```go
a := <-ch   // reads a value from ch and assigns it to a
ch <- b     // write the value in b to ch
```

Each value written to a channel can only be read once. If multiple goroutines are reading from the same channel, a value written to the channel will only be read by one of them.

### Concurrency Practices and Patterns

#### Keep your APIs Concurrency-Free

Concurrency is an implementation detail, and good API design should hide implementation details as much as possible. This allows you to change how your code works without changing how your code is invoked.

Practically, this means that you should never expose channels or mutexes in your API’s types, functions, and methods. If you expose a channel, you put the responsibility of channel management on the users of your API. This means that the users now have to worry about concerns like whether or not a channel is buffered or closed or `nil`. They can also trigger deadlocks by accessing channels or mutexes in an unexpected order.

> This doesn’t mean that you shouldn’t ever have channels as function parameters or struct fields. It means that they shouldn’t be exported.

There are some exceptions to this rule. If your API is a library with a concurrency helper function, channels are going to be part of its API.

#### The Done Channel Pattern

The done channel pattern provides a way to signal a goroutine that it’s time to stop processing. It uses a channel to signal that it’s time to exit. Let’s look at an example where we pass the same data to multiple functions, but only want the result from the fastest function:

```go
func searchData(s string, searchers []func(string) []string) []string {
    done := make(chan struct{})
    result := make(chan []string)
    for _, searcher := range searchers {
        go func(searcher func(string) []string) {
            select {
            case result <- searcher(s):
            case <-done:
            }
        }(searcher)
    }
    r := <-result
    close(done)
    return r
}
```

In our function, we declare a channel named `done` that contains data of type `struct{}`. We use an empty struct for the type because the value is unimportant; we never write to this channel, only close it. We launch a goroutine for each searcher passed in. The `select` statements in the worker goroutines wait for either a write on the `result` channel (when the `searcher` function returns) or a read on the `done` channel. Remember that a read on an open channel pauses until there is data available and that a read on a closed channel always returns the zero value for the channel. This means that the case that reads from `done` will stay paused until `done` is closed. In `searchData`, we read the first value written to `result`, and then we close `done`. This signals to the goroutines that they should exit, preventing them from leaking.

#### Using a Cancel Function to Terminate a Goroutine

```go
func countTo(max int) (<-chan int, func()) {
    ch := make(chan int)
    done := make(chan struct{})
    cancel := func() {
        close(done)
    }
    go func() {
        for i := 0; i < max; i++ {
            select {
            case <-done:
                return
            case ch<-i:

            }
        }
        close(ch)
    }()
    return ch, cancel
}

func main() {
    ch, cancel := countTo(10)
    for i := range ch {
        if i > 5 {
            break
        }
        fmt.Println(i)
    }
    cancel()
}
```

#### Turning Off a case in a select

When you need to combine data from multiple concurrent sources, the `select` keyword is great. However, you need to properly handle closed channels. If one of the cases in a `select` is reading a closed channel, it will always be successful, returning the zero value. Every time that case is selected, you need to check to make sure that the value is valid and skip the case. If reads are spaced out, your program is going to waste a lot of time reading junk values.

When that happens, we rely on something that looks like an error: reading a `nil` channel. As we saw earlier, reading from or writing to a `nil` channel causes your code to hang forever. While that is bad if it is triggered by a bug, you can use a `nil` channel to disable a `case` in a `select`. When you detect that a channel has been closed, set the channel’s variable to `nil`. The associated case will no longer run, because the read from the `nil` channel never returns a value:

```go
// in and in2 are channels, done is a done channel.
for {
    select {
    case v, ok := <-in:
        if !ok {
            in = nil // the case will never succeed again!
            continue
        }
        // process the v that was read from in
    case v, ok := <-in2:
        if !ok {
            in2 = nil // the case will never succeed again!
            continue
        }
        // process the v that was read from in2
    case <-done:
        return
    }
}
```

#### Running Code Exactly Once

Declaring a `sync.Once` instance inside a function is usually the wrong thing to do, as a new instance will be created on every function call and there will be no memory of previous invocations.

In our example, we want to make sure that parser is only initialized once, so we set the value of parser from within a closure that’s passed to the Do method on once. If Parse is called more than once, once.Do will not execute the closure again.

```go
type SlowComplicatedParser interface {
    Parse(string) string
}

var parser SlowComplicatedParser
var once sync.Once

func Parse(dataToParse string) string {
    once.Do(func() {
        parser = initParser()
    })
    return parser.Parse(dataToParse)
}

func initParser() SlowComplicatedParser {
    // do all sorts of setup and loading here
}
```

#### Channels or Mutexes

- If you are coordinating goroutines or tracking a value as it is transformed by a series of goroutines, use channels.
- If you are sharing access to a field in a struct, use mutexes.
- If you discover a critical performance issue when using channels (via Benchmarking), and you cannot find any other way to fix the issue, modify your code to use a mutex.

Mutexes in Go aren’t reentrant. If a goroutine tries to acquire the same lock twice, it deadlocks, waiting for itself to release the lock. This is different from languages like Java, where locks are reentrant.

Nonreentrant locks make it tricky to acquire a lock in a function that calls itself recursively. You must release the lock before the recursive function call. In general, be careful when holding a lock while making a function call, because you don’t know what locks are going to be acquired in those calls. If your function calls another function that tries to acquire the same mutex lock, the goroutine deadlocks.

Like `sync.WaitGroup` and `sync.Once`, mutexes must never be copied. If they are passed to a function or accessed as a field on a struct, it must be via a pointer. If a mutex is copied, its lock won’t be shared.

##### sync.Map - This is not the map you are looking for

When looking through the `sync` package, you’ll find a type called `Map`. It provides a concurrency-safe version of Go’s built-in `map`. Due to trade-offs in its implementation, `sync.Map` is only appropriate in very specific situations:

- When you have a shared `map` where key/value pairs are inserted once and read many times
- When goroutines share the `map`, but don’t access each other’s keys and values

Furthermore, because of the current lack of generics in Go, `sync.Map` uses `interface{}` as the type for its keys and values; the compiler cannot help you ensure that the right data types are used.

Given these limitations, in the rare situations where you need to share a map across multiple goroutines, use a built-in map protected by a `sync.RWMutex`.

## Testing

> adder/adder.go
```go
func addNumbers(x, y int) int {
    return x + y
}
```

> adder.adder_test.go
```go
func Test_addNumbers(t *testing.T) {
    result := addNumbers(2, 3)
    if result != 5 {
        t.Error("incorrect result: expected 5, got", result)
    }
}
```

Every test is written in a file whose name ends with *_test.go*. If you are writing tests against _foo.go_, place your tests in a file named *foo_test.go*.

- Test functions start with the word `Test` and take in a single parameter of type `*testing.T`.
- When writing unit tests for individual functions, the convention is to name the unit test `Test` followed by the name of the function. When testing unexported functions, some people use an underscore between the word `Test` and the name of the function.

The `go test` command allows you to specify which packages to test. Using `./...` for the package name specifies that you want to run tests in the current directory and all of the subdirectories of the current directory. Include a `-v` flag to get verbose testing output.

```sh-session
$ go test
PASS
ok      test_examples/adder     0.006s
```

### Reporting Test Failures

```go
t.Errorf("incorrect result: expected %d, got %d", 5, result)
```

While `Error()` and `Errorf()` make a test as failed, the test function continues running. If you think a test function should stop processing as soon as a failure is found, use the `Fatal()` and `Fatalf` methods.

The `Fatal` method works like `Error`, and the `Fatalf` method works like `Errorf`. The difference is that the test function exits immediately after the test failure message is generated.

> Note that this doesn't exit _all_ tests; any remaining test functions will execute after the current test function exits.

**When should you use `Fatal`/`Fatalf` and when should you use `Error`/`Errorf`?**

If the failure of a check in a test means that further checks in the same test function will always fail or cause the test to panic, use `Fatal` or `Fatalf`. If you are testing several independent items (such as validating fields in a `struct`), then use `Error` or `Errorf` so you can report as many problems at once. This makes it easier to fix multiple problems without rerunning your tests over and over.

### Setting Up and Tearing Down

Sometimes you have some common state that you want to set up before any tests run and remove when testing is complete. Use a `TestMain` function to manage this state and run your tests:

```go
var testTime time.Time

func TestMain(m *testing.M) {
    fmt.Println("Set up stuff for tests here")
    testTime = time.Now()
    exitVal := m.Run()
    fmt.Println("Clean up stuff after tests here")
    os.Exit(exitVal)
}

func TestFirst(t *testing.T) {
    fmt.Println("TestFirst uses stuff set up in TestMain", testTime)
}

func TestSecond(t *testing.T) {
    fmt.Println("TestSecond also uses stuff set up in TestMain", testTime)
}
```

Both `TestFirst` and `TestSecond` refer to the package-level variable `testTime`. We declare a function called `TestMain` with a parameter of type `*testing.M`. Running `go test` on a package with a `TestMain` function calls the function instead of invoking the tests directly. Once the state is configured, call the `Run` method on `*testing.M` to run the test functions. The `Run` method returns the exit code; 0 indicates that all tests passed. Finally, you must call `os.Exit` with the exit code returned from `Run`.

**Be aware that `TestMain` is invoked once, not before and after each individual test. Also be aware that you can have only one `TestMain` per package.**

There are two common situations where `TestMain` is useful:
- When you need to set up data in an external repository, such as a database
- When the code being tested depends on package-level variables that need to be initialized

#### Cleanup

The `Cleanup` method on `*testing.T` is used to clean up temporary resources created for a single test. This method has a single parameter, a function with no input parameters or return values. The function runs when the test completes. For simple tests, you can achieve the same result by using a `defer` statement, but `Cleanup` is useful when tests rely on helper functions to set up sample data.

```go
// createFile is a helper function called from multiple tests
func createFile(t *testing.T) (string, error) {
    f, err := os.Create("tempFile")
    if err != nil {
        return "", err
    }
    // write some data to f
    t.Cleanup(func() {
        os.Remove(f.Name())
    })
    return f.Name(), nil
}

func TestFileProcessing(t *testing.T) {
    fName, err := createFile(t)
    if err != nil {
        t.Fatal(err)
    }
    // do testing, don't worry about cleanup
}
```

### Storing Sample Test Data

As `go test` walks your source code tree, it uses the current package directory as the current working directory. If you want to use sample data to test functions in a package, create a subdirectory named `testdata` to hold your files. Go reserves this directory name as a place to hold test files. When reading from `testdata`, always use a relative file reference. Since `go test` changes the current working directory to the current package, each package accesses its own `testdata` via a relative file path.

```sh-session
text/
  - testdata/
    - sample1.txt
  - text.go
  - text_test.go
```

```go
package text

import "testing"

func TestCountCharacters(t *testing.T) {
	total, err := CountCharacters("testdata/sample1.txt")
	if err != nil {
		t.Error("Unexpected error:", err)
	}
	if total != 35 {
		t.Error("Expected 35, got", total)
	}
	_, err = CountCharacters("testdata/no_file.txt")
	if err == nil {
		t.Error("Expected an error")
	}
}
```

### Testing Your Public API

If you want to test just the public API of your package, Go has a convention for specifying this. You still keep your test source code in the same directory as the production source code, but you use `packagename_test` for the package name. Let’s redo our initial test case, using an exported function instead. If we have the following function in the `adder` package:

```go
func AddNumbers(x, y int) int {
    return x + y
}
```

then we can test it as public API using the following code in a file in the `adder` package named `adder_public_test.go`:

```go
package adder_test

import (
    "testing"
    "<FQDN of module>/adder"
)

func TestAddNumbers(t *testing.T) {
    result := adder.AddNumbers(2, 3)
    if result != 5 {
        t.Error("incorrect result: expected 5, got", result)
    }
}
```

Notice that the package name for our test file is `adder_test`. We have to import `test_examples/adder` even though the files are in the same directory. To follow the convention for naming tests, the test function name matches the name of the `AddNumbers` function. Also note that we use `adder.AddNumbers`, since we are calling an exported function in a different package.

Just as you can call exported functions from within a package, you can test your public API from a test that is in the same package as your source code. The advantage of using the `_test` package suffix is that it lets you treat your package as a “black box”; you are forced to interact with it only via its exported functions, methods, types, constants, and variables. Also be aware that you can have test source files with both package names intermixed in the same source directory.

### Use go-cmp to Compare Test Results

It can be verbose to write a thorough comparison between two instances of a compound type. While you can use `reflect.DeepEqual` to compare structs, maps, and slices, there’s a better way. Google released a third-party [module](https://github.com/google/go-cmp) called `go-cmp` that does the comparison for you and returns a detailed description of what does not match.

```go
type Person struct {
    Name      string
    Age       int
    DateAdded time.Time
}

func CreatePerson(name string, age int) Person {
    return Person{
        Name:      name,
        Age:       age,
        DateAdded: time.Now(),
    }
}
```

In our test file, we need to import `github.com/google/go-cmp/cmp`, and our test function looks like this:

```go
func TestCreatePerson(t *testing.T) {
    expected := Person{
        Name: "Dennis",
        Age:  37,
    }
    result := CreatePerson("Dennis", 37)
    if diff := cmp.Diff(expected, result); diff != "" {
        t.Error(diff)
    }
}
```

The `cmp.Diff` function takes in the expected output and the output that was returned by the function that we’re testing. It returns a string that describes any mismatches between the two inputs. If the inputs match, it returns an empty string. We assign the output of the `cmp.Diff` function to a variable called `diff` and then check to see if `diff` is an empty string. If it is not, an error occurred.

```go
$ go test
--- FAIL: TestCreatePerson (0.00s)
    ch13_cmp_test.go:16:   ch13_cmp.Person{
              Name:      "Dennis",
              Age:       37,
        -     DateAdded: s"0001-01-01 00:00:00 +0000 UTC",
        +     DateAdded: s"2020-03-01 22:53:58.087229 -0500 EST m=+0.001242842",
          }

FAIL
FAIL    ch13_cmp    0.006s
```

The lines with a - and + indicate the fields whose values differ. Our test failed because our dates didn’t match. This is a problem because we can’t control what date is assigned by the `CreatePerson` function. We have to ignore the `DateAdded` field. You do that by specifying a comparator function. Declare the function as a local variable in your test:

```go
comparer := cmp.Comparer(func(x, y Person) bool {
    return x.Name == y.Name && x.Age == y.Age
})
```

Pass a function to the `cmp.Comparer` function to create a customer comparator. The function that’s passed in must have two parameters of the same type and return a `bool`. It also must be symmetric (the order of the parameters doesn’t matter), deterministic (it always returns the same value for the same inputs), and pure (it must not modify its parameters). In our implementation, we are comparing the `Name` and `Age` fields and ignoring the `DateAdded` field.

Then change your call to `cmp.Diff` to include comparer:

```go
if diff := cmp.Diff(expected, result, comparer); diff != "" {
    t.Error(diff)
}
```

### Table Tests

```go
data := []struct {
    name     string
    num1     int
    num2     int
    op       string
    expected int
    errMsg   string
}{
    {"addition", 2, 2, "+", 4, ""},
    {"subtraction", 2, 2, "-", 0, ""},
    {"multiplication", 2, 2, "*", 4, ""},
    {"division", 2, 2, "/", 1, ""},
    {"bad_division", 2, 0, "/", 0, `division by zero`},
}
```

We pass two parameters to `Run`, a name for the subtest and a function with a single parameter of type `*testing.T`. Inside the function, we call `DoMath` using the fields of the current entry in `data`, using the same logic over and over. When you run these tests, you’ll see that not only do they pass, but when you use the `-v` flag, each subtest also now has a name:

```go
for _, d := range data {
    t.Run(d.name, func(t *testing.T) {
        result, err := DoMath(d.num1, d.num2, d.op)
        if result != d.expected {
            t.Errorf("Expected %d, got %d", d.expected, result)
        }
        var errMsg string
        if err != nil {
            errMsg = err.Error()
        }
        if errMsg != d.errMsg {
            t.Errorf("Expected error message `%s`, got `%s`",
                d.errMsg, errMsg)
        }
    })
}
```

### Checking Your Code Coverage

Adding the `-cover` flag to the `go test` command calculates coverage information and includes a summary in the test output. If you include a second flag `-coverprofile`, you can save the coverage information to a file:

```sh-session
go test -v -cover -coverprofile=c.out
```

The `cover` tool included with Go generates an HTML representation of your source code with that information:

```sh-session
go tool cover -html=c.out
```

The source code is in one of three colors. Gray is used for lines of code that aren’t testable, green is used for code that’s been covered by a test, and red is used for code that hasn’t been tested.

## Benchmarks

In Go, benchmarks are functions in your test files that start with the word `Benchmark` and take in a single parameter of type `*testing.B`. This type includes all of the functionality of a `*testing.T` as well as additional support for benchmarking.

```go
var blackhole int

func BenchmarkFileLen1(b *testing.B) {
    for i := 0; i < b.N; i++ {
        result, err := FileLen("testdata/data.txt", 1)
        if err != nil {
            b.Fatal(err)
        }
        blackhole = result
    }
}
```

The `blackhole` package-level variable is interesting. We write the results from `FileLen` to this package-level variable to make sure that the compiler doesn’t get too clever and decide to optimize away the call to FileLen, ruining our benchmark.

Every Go benchmark must have a loop that iterates from 0 to `b.N`. The testing framework calls our benchmark functions over and over with larger and larger values for `N` until it is sure that the timing results are accurate.

We run a benchmark by passing the `-bench` flag to go test. This flag expects a regular expression to describe the name of the benchmarks to run. Use `-bench=.` to run all benchmarks. A second flag, `-benchmem`, includes memory allocation information in the benchmark output. All tests are run before the benchmarks, so you can only benchmark code when tests pass.

> Sample benchmark run

```sh-session
BenchmarkFileLen1-12  25  47201025 ns/op  65342 B/op  65208 allocs/op
```

Running a benchmark with memory allocation information produces output with five columns.

1. _BenchmarkFileLen1-12_
    - The name of the benchmark, a hyphen, and the value of `GOMAXPROCS` for the benchmark.
2. _12_
    - The number of times that the test ran to produce a stable result.
3. _47201025 ns/op_
    - How long it took to run a single pass of this benchmark, in nanoseconds (there are 1,000,000,000 nanoseconds in a second).
4. _65342 B/op_
    - The number of bytes allocated during a single pass of the benchmark.
5. _65208 allocs/op_
    - The number of times bytes had to be allocated from the heap during a single pass of the benchmark. This will always be less than or equal to the number of bytes allocated.

> Benchmark buffers of different sizes:

```go
func BenchmarkFileLen(b *testing.B) {
    for _, v := range []int{1, 10, 100, 1000, 10000, 100000} {
        b.Run(fmt.Sprintf("FileLen-%d", v), func(b *testing.B) {
            for i := 0; i < b.N; i++ {
                result, err := FileLen("testdata/data.txt", v)
                if err != nil {
                    b.Fatal(err)
                }
                blackhole = result
            }
        })
    }
}
```

```sh-session
BenchmarkFileLen/FileLen-1-12          25  47828842 ns/op   65342 B/op  65208 allocs/op
BenchmarkFileLen/FileLen-10-12        230   5136839 ns/op  104488 B/op   6525 allocs/op
BenchmarkFileLen/FileLen-100-12      2246    509619 ns/op   73384 B/op    657 allocs/op
BenchmarkFileLen/FileLen-1000-12    16491     71281 ns/op   68744 B/op     70 allocs/op
BenchmarkFileLen/FileLen-10000-12   42468     26600 ns/op   82056 B/op     11 allocs/op
BenchmarkFileLen/FileLen-100000-12  36700     30473 ns/op  213128 B/op      5 allocs/op
```

## httptest

```go
server := httptest.NewServer(
    http.HandlerFunc(func(rw http.ResponseWriter, req *http.Request) {
        expression := req.URL.Query().Get("expression")
        if expression != io.expression {
            rw.WriteHeader(http.StatusBadRequest)
            fmt.Fprintf(rw, "expected expression '%s', got '%s'",
                io.expression, expression)
            return
        }
        rw.WriteHeader(io.code)
        rw.Write([]byte(io.body))
    }))
defer server.Close()
rs := RemoteSolver{
    MathServerURL: server.URL,
    Client:        server.Client(),
}
```

The `httptest.NewServer` function creates and starts an HTTP server on a random unused port. You need to provide an `http.Handler` implementation to process the request. Since this is a server, you must close it when the test completes. The server instance has its URL specified in the `URL` field of the server instance and a preconfigured `http.Client` for communicating with the test server.

## Integration Tests and Build Tags

The Go compiler provides _build tags_ to control when code is compiled. Build tags are specified on the first line of a file with a magic comment that starts with `// +build`. The original intent for build tags was to allow different code to be compiled on different platforms, but they are also useful for splitting tests into groups. Tests in files without build tags run all the time. These are the unit tests that don’t have dependencies on external resources. Tests in files with a build tag are only run when the supporting resources are available.

To run our integration test alongside the other tests we’ve written, use:

```sh-session
$ go test -tags integration -v ./...
```

---

> Using the -short flag

Another option is to use go test with the -short flag. If you want to skip over tests that take a long time, label your slow tests by placing the the following code at the start of the test function:

```go
if testing.Short() {
    t.Skip("skipping test in short mode.")
}
```

When you want to run only short tests, pass the `-short` flag to go test.

There are a few problems with the -short flag. If you use it, there are only two levels of testing: short tests and all tests. By using build tags, you can group your integration tests, specifying which service they need in order to run. Another argument against using the -short flag to indicate integration tests is philosophical. Build tags indicate a dependency, while the -short flag is only meant to indicate that you don’t want to run tests that take a long time. Those are different concepts.

## Finding Concurrency Problems with the Race Checker

use the flag `-race` with `go test` to enable it:

```sh-session
$ go test -race
==================
WARNING: DATA RACE
Read at 0x00c000128070 by goroutine 10:
  test_examples/race.getCounter.func1()
      test_examples/race/race.go:12 +0x45

Previous write at 0x00c000128070 by goroutine 8:
  test_examples/race.getCounter.func1()
      test_examples/race/race.go:12 +0x5b
```

You can also use the `-race flag` when you build your programs. This creates a binary that includes the race checker and that reports any races it finds to the console. This allows you to find data races in code that doesn’t have tests.

A binary with `-race` enabled runs approximately ten times slower than a normal binary. That isn’t a problem for test suites that take a second to run, but for large test suites that take several minutes, a 10x slowdown reduces productivity.
