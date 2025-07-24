# Type parameters

Starting with version 1.18, Go has added support for **generics**, also known as **type parameters**.
Go functions can be written to work on multiple types using type parameters. The type parameters of a function appear between brackets, before the function's arguments.

```go
func Index[T comparable](s []T, x T) int { }
```

This declaration means that s is a slice of any type T that fulfills the built-in constraint comparable. x is also a value of the same type.

It helps us to avoid creation of 2 similar methods:

```go
func Index(s []int, x int) int { }
func Index[T comparable](s []T, x T) int { }
```

Example:

```go
// Index returns the index of x in s, or -1 if not found.
func Index[T comparable](s []T, x T) int {
	for i, v := range s {
		// v and x are type T, which has the comparable
		// constraint, so we can use == here.
		if v == x {
			return i
		}
	}
	return -1
}

func main() {
	// Index works on a slice of ints
	si := []int{10, 20, 15, -10}
	fmt.Println(Index(si, 15)) // 2

	// Index also works on a slice of strings
	ss := []string{"foo", "bar", "baz"}
	fmt.Println(Index(ss, "hello")) // -1
}
```
