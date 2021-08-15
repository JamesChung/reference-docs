# Go Types

## Basic Types

### String types

* `string`
* `rune`    (`int32`)

### Boolean types

* `true`
* `false`

### Integer types

#### Unsigned types
* `uint`    (usually based on CPU arch)
* `uint8`   (`byte`)
* `uint16`
* `uint32`
* `uint64`
* `uintptr` (usually based on CPU arch)

#### Signed types

* `int`     (usually based on CPU arch)
* `int8`
* `int16`
* `int32`   (`rune`)
* `int64`

### Floating Point types

* `float32`
* `float64`

### Complex types

* `complex64`
* `complex128`

## Aggregate types

* `[...]array`
* `struct{}`

## Reference types

* Slice - `[]type`
* Channel - `chan type`
* Pointer - `*type`
* Map - `map[type]type`
* Function - `func(type...)type...`
* Interface - `interface{}`
* Invalid - `reflect.Invalid`
