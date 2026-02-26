# Arrays

An **array** is a collection of values that all share the same type.

To declare a variable that holds an array, you need to specify the number of elements it holds in square brackets (`[]`), followed by the type of elements the array holds:

```go
var x [3]int // declare a variable `x` as an array of 3 integers.
```

When an array is created, all the values it contains are initialized to the zero value for the type the array holds.

An array's length is part of its type, so arrays *cannot be resized*.

```go
var x [2]string
x[0] = "Good"
x[1] = "morning"
fmt.Println(x[0], x[1]) // Good morning
fmt.Println(x) // [Good morning]
```

## Array literals

If you know in advance what values an array should hold, you can initialize the array with those values using an array literal. 
An array literal starts just like an array type, with the number of elements it will hold in square brackets, followed by the type of its elements.

```go
nums := [3]int{2, 3, 5}
fmt.Println(nums) // [2 3 5]
```
