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

The main reason arrays exist in Go is to provide the backing store for slices, which are one of the most useful features of Go.

## Array literals

If you know in advance what values an array should hold, you can initialize the array with those values using an array literal. 
An array literal starts just like an array type, with the number of elements it will hold in square brackets, followed by the type of its elements.

```go
nums := [3]int{2, 3, 5}
fmt.Println(nums) // [2 3 5]
```

## Accessing array elements

```go
numbers := [3]int{11, 22, 33}

for i := 0; i < 3; i++ {
  fmt.Println(numbers[i])
}
```

Arrays hold a specific number of elements. Trying to access an index that is outside the array will cause a **panic**, 
an error that occurs while your program is running (as opposed to when it's compiling).

```go
numbers := [2]int{11, 22}

for i := 0; i < 3; i++ {
  fmt.Println(numbers[i]) // panic: runtime error: index out of range [2] with length 2
}
```

## `len()` function

```go
notes := [7]string{"Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"}

fmt.Println(len(notes)) // 7
```

## Looping over arrays with `for ... range`

```go
notes := [7]string{"Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"}

for index, value := range notes {
  fmt.Println(index, value)
}
```

## Multidimensional arrays

```go
arr := [3][2]int{{1, 2}, {3, 4}, {5, 6}}

fmt.Printf("%T\n", arr) // [3][2]int

for _, row := range arr {
  fmt.Println(row)
}
/*
[1 2]
[3 4]
[5 6]
*/
```

We can use `[...]` and compiler will set the array's size automatically:

```go
arr := [...][2]int{{1, 2}, {3, 4}, {5, 6}}
fmt.Printf("%T\n", arr) // [3][2]int
```

## Sparse arrays

A sparse array is a data structure designed to store large arrays containing mostly default values (e.g., zero or null) by only storing non-zero elements and their positions. 
This technique significantly reduces memory usage and improves computation speed.

```go
var x = [...]int{1, 5: 5, 6, 10: 10, 11, 12}

fmt.Println(x) // [1 0 0 0 0 5 6 0 0 0 10 11 12]
```
