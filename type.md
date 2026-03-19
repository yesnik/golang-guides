# Type

## Defining methods

- A method definition is very similar to a function definition. 
In fact, there's really only one difference: we add one extra parameter, a *receiver parameter*, in parentheses before the function name.
- The value we are calling the method on is known as the *method receiver*.
- The method we are defining becomes associated with all values of that type.
- We can access contents of the receiver parameter within the method block.
- Go lets you name a receiver parameter whatever you want. By convention, Go developers usually use a name consisting of a single letter -
   the first letter of the receiver's type name, in lowercase. (This is why we used `c` as the name for our `CurrencyName` receiver parameter.)
- Go uses receiver parameters instead of the `self` in Python or `this` in Javascript.

Below, we define a type named `CurrencyName`, with an underlying type of `string`.
Then, we define a method named `getName`. Because `getName` has a receiver parameter with a type of `CurrencyName`, 
we'll be able to call the `getName` method on any `CurrencyName` value. 

Here we can say that `getName` is defined "on" `CurrencyName`.

```go
// Define a type with an underlying type of "string"
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

## Receiver parameter

### A copy of the receiver

Like any other parameter, a receiver parameter receives a copy of the receiver value. 
If we make changes to the receiver within a method, we're changing the copy, not the original.

```go
type Number int

func (n Number) Double() {
	n *= 2
}

number := Number(5)
fmt.Println("Original value:", number) // Original value: 5
number.Double()
fmt.Println("Doubled value:", number) // Doubled value: 5
```

### A pointer type of the receiver

```go
// Change the receiver parameter to a pointer type
func (n *Number) Double() {
	// Update the value at the pointer
	*n *= 2
}

number := Number(5)
fmt.Println("Original value: ", number) Original value: 5
number.Double() // <-- No changes here
fmt.Println("Doubled value: ", number) Doubled value: 10
```

Notice that we didn't have to change the method call at all.

### Go converts "value receiver" <-> "pointer receiver"

When we call a method that requires a *pointer receiver* on a variable with a nonpointer type, 
Go will automatically convert the receiver to a pointer for you. 

If we call a method requiring a value receiver, Go will automatically get the value at the pointer for you and pass that to the method.

```go
type MyType string

func (m MyType) method() {
	fmt.Println("Method with value receiver")
}
func (m *MyType) pointerMethod() {
	fmt.Println("Method with pointer receiver")
}

value := MyType("value")
pointer := &value
value.method() // Method with value receiver
value.pointerMethod() // Method with pointer receiver
pointer.method() // Method with value receiver
pointer.pointerMethod() // Method with pointer receiver
```

The method named `method` takes a value receiver, but we can call it using both direct values and pointers, because Go autoconverts if needed. 

And the method named `pointerMethod` takes a pointer receiver, but we can call it on both direct values and pointers, because Go will autoconvert if needed.

**Convention**: For consistency, all of our type's methods can take *value* receivers, or they can all take *pointer* receivers, but we should avoid mixing the two. 
