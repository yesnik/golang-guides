# Methods

Go does *not have classes*, but we can define methods on types.

A method is a function with a special *receiver argument*.

The receiver appears in its own argument list between the func keyword and the method name.

In this example, the `FullName` method has a receiver of type `Student` named `s`.

```go
type Student struct {
	firstname, lastname string
}

func (s Student) FullName() string {
	return s.firstname + " " + s.lastname
}

func main() {
	s := Student{"Kenny", "Smith"}
	fmt.Println(s.FullName()) // Kenny Smith
}
```

Method is *just a function* with a receiver argument. We can rewrite `FullName` as a regular function with no change in functionality:

```go
type Student struct {
	firstname, lastname string
}

func FullName(s Student) string {
	return s.firstname + " " + s.lastname
}

func main() {
	s := Student{"Kenny", "Smith"}
	fmt.Println(FullName(s)) // Kenny Smith
}
```

## Declare a method on non-struct types

We can declare a method on non-struct types.

We can only declare a method with a receiver whose type is defined in the same package as the method. 
We can't declare a method with a receiver whose type is defined in another package (which includes the built-in types such as int).

In this example we see a string type `MyString` with an `Concat` method.

```go
type MyString string

func (a MyString) Concat(b string) string {
	return string(a) + b
}

func main() {
	s := MyString("Hello")
	fmt.Println(s.Concat(" world")) // Hello world
}
```
