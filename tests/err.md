These tests cover error reporting.

### Preliminary setup

```
>>> let prices = load("sample_csv/prices.csv")

>>> var my_int = 0

```

### Parse errors

```
>>> 'Hello'
Error: character must have exactly one item

>>> join prices, events on symbol on name
Error: 'on' already listed

```

These messages should be improved.

```
>>> from prices select where symbol == "AAPL", volume > 30000000
Error: unable to parse

>>> 1 +
Error: unable to parse

>>> (
Error: unable to parse

```

### Type errors

```
>>> 1 + "2"
Error: unable to match overloaded function +
  candidate: (Int64, Int64) -> Int64
    argument type at position 1 does not match: String vs Int64
  candidate: (Float64, Float64) -> Float64
    argument type at position 0 does not match: Int64 vs Float64
  candidate: (Int64, Float64) -> Float64
    argument type at position 1 does not match: String vs Float64
  ...
  <53 others>

>>> my_int = 7.
Error: mismatched types in assignment: Int64 vs Float64

```

### Identifier errors

```
>>> my_float = 0.0
Error: symbol my_float was not found

```

### Assignment errors

```
>>> let x = 7

>>> x = 8
Error: target of assignment is read only

>>> func foo(): return 1 end

>>> foo = 7
Error: target of assignment is read only

>>> 3 = 7
Error: target of assignment cannot be temporary

>>> var y = 7

>>> y = 8.0
Error: mismatched types in assignment: Int64 vs Float64

>>> y = print("Hello")
Error: type 'void' is not assignable

>>> var z
Error: unable to determine type for z

>>> let $ m = y
Error: macro parameter m requires a comptime literal value

```

### Functions

```
>>> x{3}
Error: type Int64 is not a template
Error: wrong number of arguments; expected 0 but got 1

>>> func add($ a: Int64, b: Int64) => a + b

>>> add(y, 7)
Error: macro parameter a requires a comptime literal

>>> func misspelled{x:Int64}(y:int64) = x + y

>>> misspelled{1}(2)
Error: symbol int64 was not found
Error: declaration for y has invalid type

```

### User-defined types

```
>>> data D = 1 + 2
Error: cannot assign D to a value

>>> data D: name: String, age end
Error: unable to determine type for D.age

```

### Generics

```
>>> func gen_add[T](a: T, b: T) = a + b

>>> gen_add(4, 5.)
Error: argument type at position 1 does not match: Float64 vs T aka Int64

>>> gen_add([1, 2], [3., 4.])
Error: argument type at position 1 does not match: [Float64] vs T aka [Int64]

>>> func gen_add2[T](a: [T], b: T) = a + b

>>> gen_add2([1, 2, 3], [4, 5, 6])
Error: argument type at position 1 does not match: [Int64] vs T aka Int64

>>> gen_add2(4, 5)
Error: argument type at position 0 does not match: Int64 vs [T]

```
