# Type

```go
type CurrencyName string

func (c CurrencyName) getName() string {
	if c == "EUR" {
		return "Euro"
	}
	return "Unknown"
}

func main() {
	value := CurrencyName("EUR")
	result := value.getName()

	fmt.Println(result) // Euro
}
```
