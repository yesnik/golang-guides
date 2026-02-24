# Functions

A parameter is a variable, local to a function, whose value is set when the function is called.

When the function is run, each parameter will be set to *a copy of the value* in the corresponding argument. 
Those parameter values are then used within the code in the function block.

```go
func repeatLine(line string, times int) {
	for i := 0; i < times; i++ {
		fmt.Println(line)
	}
}

repeatLine("Hello", 100)
```

## Omit the type

In this example, we shortened `a int, b int` to `a, b int`:

```go
func add(a, b int) int {
	return a + b
}
```

## Variable scope

Variables declared outside a function block will be in scope within that block. 
That means we can declare a variable at the package level, and access it within any function in that package.

```go
import (
	"fmt"
)

var metersPerLiter float64

func paintNeeded(width float64, height float64) float64 {
	return width * height / metersPerLiter
}

func main() {
	metersPerLiter = 10.5
	fmt.Printf("%.2f liters needed\n", paintNeeded(4.3, 3.5))
}
```

## A function can return any number of results

Go allows multiple return values from a function or method call.

This function returns 3 values:

```go
func manyReturns() (string, int, bool) {
	return "Hi", 5, true
}

func main() {
	myString, myInt, myBool := manyReturns()
	fmt.Println(myString, myInt, myBool) // Hi 5 true
}
```

One common use of multiple return values is to return the function's main result, and then a second value indicating whether there was an error.

```go
import (
	"bufio"
	"fmt"
	"log"
	"os"
)

func main() {
	reader := bufio.NewReader(os.Stdin)
	fmt.Print("Type something: ")
	input, err := reader.ReadString('\n')
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(input)
}
```

## Ignore the error with the blank indentifier

The blank identifier `_` can be used in place of any variable in any assignment statement.

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

The main purpose of named return values is as documentation for programmers reading the code.

```go
func floatParts(num float64) (integerPart int, fractionalPart float64) {
	integerNum := math.Floor(num)
	
	return int(integerNum), num - integerNum
}

func main() {
	intPart, remainter := floatParts(3.14)
	fmt.Println(intPart, remainter) // 3 0.14000000000000012
}
```

### "naked" return 

It returns the named return values.

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
