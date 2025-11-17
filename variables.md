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

We can use the `var` keyword with type inference and initializer.
Go can infer the type of the variable from the initial value assigned. In this case, the type can be omitted:

```go
var a = 1 // Go infers 'a' is of type int

func main() {
	var active, name = true, "Kenny"
	fmt.Println(a, active, name) // Output: 1 true Kenny
}
```

## Short assignment statement `:=`

This is the most common and concise way to declare and initialize variables within a function in Go. 
It automatically infers the type of the variable and assigns the initial value.

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

Outside a function, every statement begins with a keyword (var, func, and so on) and so the `:=` construct is not available.

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

