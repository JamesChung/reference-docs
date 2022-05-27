# Swift Reference

## `typealias`

```swift
typealias VoidVoidFunction = () -> ()

func doThis(_ f: VoidVoidFunction) {
    f()
}
```

## Anonymous Functions

```swift
{
    () -> () in
    // Function body code below...
    print("Hello World!")
}
```

```swift
{
    (x: Int, y: Int) -> Int in
    return x + y
}
```

### Inline anonymous function

```swift
func invokeFunc(x: Int, y: Int, _ f:(Int, Int) -> Int) {
    print(f(x, y))
}

invokeFunc(x: 10, y: 20, {
    (_ x: Int, _ y: Int) -> Int in
    return x + y
})
```

### Omission of the return type

> If the anonymous function's return type is already known to the compiler, you
> can omit the arrow operator and the specification of the return type

```swift
func invokeFunc(x: Int, y: Int, _ f:(Int, Int) -> Int) {
    print(f(x, y))
}

invokeFunc(x: 10, y: 20, {
    (_ x: Int, _ y: Int) in
    return x + y
})
```

### Omission of the `in` expression when there are no parameters

> If the anonymous function takes no parameters, and if the return type can be
> omitted, the `in` expression itself can be omitted

```swift
func invokeFunc(_ f:() -> ()) {
    f()
}

invokeFunc({
    print("Anonymous says hello")
})
```

### Omission of the parameter types

> If the anonymous function takes parameters and their types are already known
> to the compiler, the types can be omitted

```swift
func invokeFunc(x: Int, y: Int, _ f:(Int, Int) -> Int) {
    print(f(x, y))
}

invokeFunc(x: 10, y: 20, {
    (x, y) in
    return x + y
})
```

### Omission of the parentheses

> If the parameter types are omitted, the parentheses around the parameter list
> can be omitted

```swift
func invokeFunc(x: Int, y: Int, _ f:(Int, Int) -> Int) {
    print(f(x, y))
}

invokeFunc(x: 10, y: 20, {
    x, y in
    return x + y
})
```

### Omission of the `in` expression when there are parameters

> If the return type can be omitted, and if the parameter types are already
> known to the compiler, you can omit the `in` expression and refer to the
> parameters directly within the body of the anonymous function by using the
> magic names `$0`, `$1`, and so on, in order

```swift
func invokeFunc(x: Int, y: Int, _ f:(Int, Int) -> Int) {
    print(f(x, y))
}

invokeFunc(x: 10, y: 20, {
    return $0 + $1
})
```
