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
