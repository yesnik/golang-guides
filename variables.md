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
