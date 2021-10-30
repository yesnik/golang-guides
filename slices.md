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

Changing the elements of a slice modifies the corresponding elements of its underlying array.

Other slices that share the same underlying array will see those changes.

```go
letters := [4]string{"A", "B", "C", "D"}
fmt.Println(letters) // [A B C D]

a := letters[0:2] // [A B]
b := letters[1:3] // [B C]
fmt.Println(a, b) // [A B] [B C]

b[0] = "ZZZ"
fmt.Println(a, b) // [A ZZZ] [ZZZ C]
fmt.Println(letters) // [A ZZZ C D]
```

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

- length: the number of elements it contains. Function: `len(slice)`
- capacity: the number of elements in the underlying array, counting from the first element in the slice. Function: `cap(slice)`

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
