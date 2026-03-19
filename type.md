# Type

## Defining methods

- A method definition is very similar to a function definition. 
In fact, there's really only one difference: we add one extra parameter, a *receiver parameter*, in parentheses before the function name.
- The value we are calling the method on is known as the *method receiver*.
- The method we are defining becomes associated with all values of that type.

Below, we define a type named `CurrencyName`, with an underlying type of `string`.
Then, we define a method named `getName`. Because `getName` has a receiver parameter with a type of `CurrencyName`, 
we'll be able to call the `getName` method on any `CurrencyName` value. 

Here we can say that `getName` is defined "on" `CurrencyName`.

```go
type CurrencyName string

// "c" is a receiver parameter name
// "CurrencyName" is a receiver parameter type
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
