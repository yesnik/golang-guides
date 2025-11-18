# Functions

## Omit the type

In this example, we shortened `a int, b int` to `a, b int`:

```go
func add(a, b int) int {
	return a + b
}
```

## A function can return any number of results

This function returns 3 strings:

```go
func swap(a, b, c string) (string, string, string) {
	return c, b, a
}

func main() {
	a, b, c := swap("one", "two", "three")
	fmt.Println(a, b, c) // Output: three two one
}
```
## Ignore the error with the blank indentifier

`_` is a blank identifier.

```go
import (
	"fmt"
	"bufio"
	"os"
)

func main() {
	filePath := "file.txt" // Replace with your file path

	content, _ := os.ReadFile(filePath)

	fmt.Printf("File content:\n%s\n", content)
}
```

## Named return values

Go's return values may be named. If so, they are treated as variables defined at the top of the function.
Here we can see a *"naked" return*. It returns the named return values.
```go
func split(sum int) (a, b int) {
	a = sum - 5
	b = sum - a
	return
}

func main() {
	fmt.Println(split(12)) // Output: 7 5
}
```

## Function values

Functions are values too. They can be passed around just like other values.

Function values may be used as function arguments and return values.

```go
// Here we define typehint for `fn` argument
func compose(fn func(string, string) string) string {
	return fn("Hello", "World")
}

func main() {
	question_add := func(x, y string) string {
		return x + " " + y + "?"
	}
	fmt.Println(question_add("Hi", "Kenny")) // Hi Kenny?

	fmt.Println(compose(question_add)) // Hello World?
	
	exclamation_point_add := func(x, y string) string {
		return x + " " + y + "!"
	}
	fmt.Println(compose(exclamation_point_add)) // Hello World!
}
```

## Function closures

A *closure* is a function value that references variables from outside its body. 
The function may access and assign to the referenced variables. We can say that the function is "bound" to the variables.

For example, the `counter` function returns a closure. Each closure is bound to its own variable - `up` and `down`.

```go
func counter() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	up, down := counter(), counter()
	for i := 0; i < 3; i++ {
		fmt.Println(
			up(i),
			down(-i),
		)
	}
}
/* Output:
0 0
1 -1
3 -3
*/
```

### Fibonacci exercise

Implement a fibonacci function that returns a function (a closure) that returns successive fibonacci numbers (0, 1, 1, 2, 3, 5, ...).

```go
// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
	// We need to set a = 1, b = 0, to ..
	a, b := 1, 0
	return func() int {
		// .. make a == 0 in the first iteration
		a, b = b, a + b
		return a
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```
