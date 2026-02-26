# Arrays

An **array** is a collection of values that all share the same type.

`var x [3]int` - declare a variable `x` as an array of 3 integers.

An array's length is part of its type, so arrays *cannot be resized*.

```go
var x [2]string
x[0] = "Good"
x[1] = "morning"
fmt.Println(x[0], x[1]) // Good morning
fmt.Println(x) // [Good morning]

nums := [3]int{2, 3, 5}
fmt.Println(nums) // [2 3 5]
```
