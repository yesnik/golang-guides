# Functions

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
