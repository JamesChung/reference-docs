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
* [Universe Block](#Universe-Block)
* [Pointer Performance](#Pointer-Performance)
* [Embedding](#Embedding)
  * [Embedding Interfaces](#Embedding-Interfaces)

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

---

## [Universe Block](https://golang.org/ref/spec#Predeclared_identifiers)

```md
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
