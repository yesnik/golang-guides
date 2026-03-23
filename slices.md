# Slices

An array has a fixed size. A *slice* is a dynamically-sized, flexible view into the elements of an array. 
In practice, slices are much more common than arrays.

```go
nums := [6]int{2, 4, 6, 8, 10, 12} // array

var s []int = nums[2:4] // create slice
fmt.Println(s) // [6 8]

letters := []string{"A", "B"}
fmt.Println(letters) // [A B]
```

## Slices are like references to arrays

A slice does not store any data, it just describes a section of an underlying array.

Every slice is built on top of an *underlying array*. 
It's the underlying array that actually holds the slice's data; 
the slice is merely *a view* into some (or all) of the array's elements.

When you use the `make` function or a slice literal to create a slice, the underlying array is created for you automatically (and you can't access it, except through the slice).
But you can also create the array yourself, and then create a slice based on it with the slice operator.

### Change the underlying array

If you change the underlying array, those changes will also be visible within the slice:

```go
letters := [5]string{"A", "B", "C", "D", "E"}
fmt.Println(letters) // [A B C D E]

slice := letters[0:3]
letters[2] = "XXX" // Change an element of the underlying array
fmt.Println(letters) // [A B XXX D E]
fmt.Println(slice) // [A B XXX]
```

If multiple slices point to the same underlying array, a change to the array's elements will be visible in all the slices:

```go
letters := [5]string{"A", "B", "C", "D", "E"}
fmt.Println(letters) // [A B C D E]

sliceOne := letters[:3]
sliceTwo := letters[2:]
letters[2] = "XXX" // Change an element of the slice
fmt.Println(letters) // [A B XXX D E]
fmt.Println(sliceOne) // [A B XXX]
fmt.Println(sliceTwo) // [XXX D E]
```

### Change the slice

Assigning a new value to a slice element will change the corresponding element in the underlying array:

```go
letters := [5]string{"A", "B", "C", "D", "E"}
fmt.Println(letters) // [A B C D E]

slice := letters[0:3]
slice[2] = "XXX" // Change an element of the slice
fmt.Println(letters) // [A B XXX D E]
fmt.Println(slice) // [A B XXX]
```

**Note**: With `make` and with slice literals, you never have to work with the underlying array.

## Slice literals

A slice literal is like an array literal without the length.

This is an array literal: `[3]bool{true, true, false}`

And this creates the same array as above, then builds a slice that references it: `[]bool{true, true, false}`

```go
nums := []int{1, 2, 3}
fmt.Println(nums) // [1 2 3]

r := []bool{true, false}
fmt.Println(r) // [true false]

s := []struct {
  i int
  b bool
}{
  {1, true},
  {2, false},
}
fmt.Println(s) // [{1 true} {2 false}]
```

## Slice defaults

When slicing, we can omit the high or low bounds to use their defaults instead.

The default is zero for the low bound and the length of the slice for the high bound.

For the array `var a [5]int` these slice expressions are equivalent:

```go
a[0:5]
a[:5]
a[0:]
a[:]
```

```go
s := []int{2, 4, 6, 8, 10}

s1 := s[1:4]
fmt.Println(s1) // [4 6 8]

s2 := s[:3]
fmt.Println(s2) // [2 4 6]

s3 := s[2:]
fmt.Println(s3) // [6 8 10]
```

## Slice length and capacity

A slice has:

- **length**: the number of elements it contains. Function: `len(slice)`
- **capacity**: the number of elements in the underlying array, counting from the *first element* in the slice. Function: `cap(slice)`

You can extend a slice's length by re-slicing it, provided it has sufficient capacity. 

If we'll try to extend slice beyond its capacity, error will occur: *panic: runtime error: slice bounds out of range [:10] with capacity 5*

```go
func main() {
	s := []int{2, 4, 6, 8, 10}
	printSlice(s) // [2 4 6 8 10] len=5 cap=5

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s) // [] len=0 cap=5

	// Extend its length.
	s = s[:4]
	printSlice(s) // [2 4 6 8] len=4 cap=5

	// Drop its first two values.
	s = s[2:]
	printSlice(s) // [6 8] len=2 cap=3
}

func printSlice(s []int) {
	fmt.Printf("%v len=%d cap=%d\n", s, len(s), cap(s))
}
```

## Nil slices

The zero value of a slice is `nil`. A `nil` slice has a length and capacity of `0` and has no underlying array.

```go
var s []int
fmt.Println(s, len(s), cap(s)) // [] 0 0
fmt.Println(s == nil) // true
```

## Create a slice with make

Function `make` helps to create dynamically-sized arrays.

The `make` function allocates a zeroed array and returns a slice that refers to that array:

```go
func main() {
	a := make([]int, 5)
	printSlice(a) // [0 0 0 0 0] len=5 cap=5

	b := make([]int, 0, 5) // Third argument is capacity
	printSlice(b) // [] len=0 cap=5

	c := b[:2]
	printSlice(c) // [0 0] len=2 cap=5

	d := c[2:5]
	printSlice(d) // [0 0 0] len=3 cap=3
}

func printSlice(x []int) {
	fmt.Printf("%v len=%d cap=%d\n", x, len(x), cap(x))
}
```

## Slices of slices

Slices can contain any type, including other slices.

```go
import (
	"fmt"
	"strings"
)

func main() {
	board := [][]string{
		[]string{"_", "_"},
		[]string{"_", "_"},
	}
	
	fmt.Println(board) // [[_ _] [_ _]]

	board[0][0] = "1"
	board[0][1] = "2"
	board[1][0] = "3"
	board[1][1] = "4"

	for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}
/*
1 2
3 4
*/
```

## A slice as an argument is updated

A slice has a pointer to an underlying array. In the `modify` function we're changing underlying array.

```go
func modify(arr []string) {
	arr[0] = "updated"
}

s := []string{"a", "b"}

modify(s)

fmt.Println(s) // [updated b]
```

## Appending to a slice

Function `append` allows us to append new elements to a slice.

The resulting value of append is a slice containing all the elements of the original slice plus the provided values.

If the backing array of `s` is too small to fit all the given values a bigger array will be allocated. 
The returned slice will point to the newly allocated array.

```go
func main() {
	var s []int
	printSlice(s) // [] len=0 cap=0

	// append works on nil slices.
	s = append(s, 1)
	printSlice(s) // [1] len=1 cap=1

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s) // [1 1] len=2 cap=2

	// We can add more than one element at a time.
	s = append(s, 1, 2)
	printSlice(s) // [1 1 1 2] len=4 cap=4
}

func printSlice(s []int) {
	fmt.Printf("%v len=%d cap=%d\n", s, len(s), cap(s))
}
```

When calling `append`, it's conventional to just assign the return value back to the same slice variable you passed to append. 
You don't need to worry about whether two slices have the same underlying array if you're only storing one slice.

### Different underlying array

When we assign a value to an element of the `s4` slice, we can see the change reflected in `s3`, because `s4` and `s3` happen to share the same underlying array. 
But the change is not reflected in `s2` or `s1`, because they have a different underlying array.

```go
s1 := []string{"s1", "s1"}
s2 := append(s1, "s2", "s2")
s3 := append(s2, "s3", "s3")
s4 := append(s3, "s4", "s4")

fmt.Println(s1, s2, s3, s4)
// [s1 s1] [s1 s1 s2 s2] [s1 s1 s2 s2 s3 s3] [s1 s1 s2 s2 s3 s3 s4 s4]
s4[0] = "NN"

fmt.Println(s1, s2, s3, s4)
// [s1 s1] [s1 s1 s2 s2] [NN s1 s2 s2 s3 s3] [NN s1 s2 s2 s3 s3 s4 s4]
```
