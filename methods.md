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

## Methods with pointer receivers

Methods with pointer receivers can modify the value to which the receiver points. 
Since methods often need to modify their receiver, pointer receivers are more common than value receivers.

```go
type Student struct {
	firstName, lastName string
}

func (s Student) FullName() string {
	return s.firstName + " " + s.lastName
}

func (s *Student) ChangeFirstName(firstName string) {
	s.firstName = firstName
}

func main() {
	s := Student{"Joe", "Doe"}
	s.ChangeFirstName("Larry")
	fmt.Println(s.FullName()) // Larry Doe
}
```

## Value receiver VS pointer receiver

There are two reasons to use a pointer receiver.

1. The method can modify the value that its receiver points to.
2. Avoid copying the value on each method call. This can be more efficient if the receiver is a large struct.

In general, all methods on a given type should have either value or pointer receivers, but not a *mixture of both*.

In this example, both `Scale` and `Area` are with receiver type `*Square`, even though the `Area` method needn't modify its receiver.

```go
type Square struct {
	A float64
}

func (s *Square) Scale(f float64) {
	s.A = s.A * f
}

func (s *Square) Area() float64 {
	return s.A * s.A
}

func main() {
	s := Square{3}
	fmt.Printf("%s, Area: %v\n", s, s.Area()) // {%!s(float64=3)}, Area: 9
	s.Scale(2)
	fmt.Printf("%s, Area: %v\n", s, s.Area()) // {%!s(float64=6)}, Area: 36
}
```
