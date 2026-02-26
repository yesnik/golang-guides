# Contants

```go
const DaysInWeek int = 7
const DaysInWeek = 7
```

- Constants are declared like variables, but with the `const` keyword.
- Constants can be character, string, boolean, or numeric values.
- Constants cannot be declared using the `:=` syntax.
- It's possible to declare a constant inside a function, that would limit its scope to the block for that function.
- It's much more typical to declare constants at the package level, so they can be accessed by all functions in that package.

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
