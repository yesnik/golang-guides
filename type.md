# Type

## Defining methods

- A method definition is very similar to a function definition. 
In fact, there's really only one difference: we add one extra parameter, a *receiver parameter*, in parentheses before the function name.
- The value we are calling the method on is known as the *method receiver*.
- The method we are defining becomes associated with all values of that type.
- We can access contents of the receiver parameter within the method block.
- Go lets you name a receiver parameter whatever you want. By convention, Go developers usually use a name consisting of a single letter -
   the first letter of the receiver's type name, in lowercase. (This is why we used `c` as the name for our `CurrencyName` receiver parameter.)

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
