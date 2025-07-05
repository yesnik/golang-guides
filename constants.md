# Contants

- Constants are declared like variables, but with the `const` keyword.
- Constants can be character, string, boolean, or numeric values.
- Constants cannot be declared using the `:=` syntax.

```go
const G = 9.81

func main() {
	const World = "Earth"
	
	fmt.Println("Hello", World) // Output: Hello Earth
	fmt.Println("Acceleration", G) // Output: Acceleration 9.81

	const Active = true
	fmt.Println("Active?", Active) // Output: Active? true
}
```
