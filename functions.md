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
