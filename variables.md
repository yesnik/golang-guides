# Variables

## `var` statement

The `var` statement declares a list of variables, the type is last. A `var` statement can be at package or function level.

```go
var a, b int

func main() {
	var active bool
	fmt.Println(a, b, active) // Output: 0 0 false
}
```

## Variables with initializers

If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

```go
var a, b int = 1, 2

func main() {
	var active, name = true, "Kenny"
	fmt.Println(a, b, active, name) // Output: 1 2 true Kenny
}
```

## Short assignment statement `:=`

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

Outside a function, every statement begins with a keyword (var, func, and so on) and so the := construct is not available.

```go
x := 1 // Syntax error: non-declaration statement outside function body
var y = 1 // No error

func main() {
	var a, b int = 1, 2
	c := 3
	
	active, name := true, "John"

	fmt.Println(a, b, c, active, name) // Output: 1 2 3 true John
}
```

