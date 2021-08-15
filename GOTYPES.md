# Go Types

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
