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