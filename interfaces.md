# Interfaces

An interface type is defined as a set of method signatures.

A type implements an interface *by implementing its methods*. There is *no explicit declaration of intent*, no "implements" keyword.

Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

```go
import "fmt"

type Info interface {
	GetInfo()
}

type Hero struct {
	Name string
}

// This method means type Hero implements the interface Info,
// but we don't need to explicitly declare that it does so.
func (h Hero) GetInfo() {
	fmt.Println("Name: " + h.Name)
}

func main() {
	var i Info = Hero{"Robocop"}
	i.GetInfo() // Name: Robocop
}
```

Below `v` is a `Vertex` (not `*Vertex`) and does NOT implement `Abser` interface. There will be an error:

```go

import (
	"fmt"
	"math"
)

type Abser interface {
	Abs() float64
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}


func main() {
	f := MyFloat(-4.0)
	fmt.Println(f) // -4
	fmt.Println(f.Abs()) // 4
	
	v := Vertex{3, 4}

	var a Abser
	a = f  // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser

	// In the following line, v is a Vertex (not *Vertex)
	// and does NOT implement Abser:
	a = v // Error!

	fmt.Println(a.Abs()) // 5
}
```

## Interface values

Under the hood, interface values can be thought of as a tuple of a value and a concrete type:

```
(value, type)
```

An interface value holds a value of a specific underlying concrete type.

Calling a method on an interface value executes the method of the same name on its underlying type.

```go
type Shape interface {
	getArea()
}

type Square struct {
	A int
}

func (square Square) getArea() {
	fmt.Println(square.A * square.A)
}


type Rectangle struct {
	A, B int
}

func (rectangle Rectangle) getArea() {
	fmt.Println(rectangle.A * rectangle.B)
}


func main() {
	var square Shape
	square = Square{2}
	square.getArea() // 4
	
	var rectangle Shape
	rectangle = Rectangle{2, 3}
	rectangle.getArea() // 6
}
```

### Interface values with nil underlying values

If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.

```go
type Flyable interface {
	Fly()
}

type Hero struct {
	Name string
}

func (hero *Hero) Fly() {
	if hero == nil {
		fmt.Println("Can't fly - Hero is <nil>")
		return
	}
	fmt.Println(hero.Name + " is flying")
}

func main() {
	var flyablePerson Flyable
	var hero *Hero
	
	flyablePerson = hero
	flyablePerson.Fly() // Can't fly - Hero is <nil>
	
	flyablePerson = &Hero{"Superman"}
	flyablePerson.Fly() // Superman is flying
}
```

A nil interface value holds neither value nor concrete type.

Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which concrete method to call.

```go
type I interface {
	M()
}

func main() {
	var i I
	i.M() // panic: runtime error: invalid memory address or nil pointer dereference
              // [signal SIGSEGV: segmentation violation code=0x1 addr=0x0 pc=0x477970]
}
```

## The empty interface

The interface type that specifies zero methods is known as the *empty interface*:
```go
interface{}
```

An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. 
For example, `fmt.Print` takes any number of arguments of type `interface{}`.

```go
func main() {
	var i interface{}
	describe(i) // (<nil>, <nil>)

	i = 18
	describe(i) // (18, int)

	i = "hey"
	describe(i) // (hey, string)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

## Type assertions

```go
t := i.(T)
```

This statement asserts that the interface value i holds the concrete type T and assigns the underlying T value to the variable t.

If `i` does not hold a `T`, the statement will trigger a panic.

To test whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

```go
t, ok := i.(T)
```

```go
func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s) // hello

	s, ok := i.(string)
	fmt.Println(s, ok) // hello true

	f, ok := i.(float64)
	fmt.Println(f, ok) // 0 false

	f = i.(float64) // panic
	fmt.Println(f)
}
```

## Type switches

*A type switch* is a construct that permits several type assertions in series.

```go
func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("Unknown type %T!\n", v)
	}
}

func main() {
	do(18) // Twice 18 is 36
	do("hello") // "hello" is 5 bytes long
	do(true) // Unknown type bool!
}
```

## Stringers

One of the most ubiquitous interfaces is Stringer defined by the `fmt` package.

```go
type Stringer interface {
    String() string
}
```

A `Stringer` is a type that can describe itself as a string. The `fmt` package (and many others) look for this interface to print values.

```go
type Robot struct {
	Name string
	Power  int
}

func (p Robot) String() string {
	return fmt.Sprintf("%v, power %v", p.Name, p.Power)
}

func main() {
	a := Robot{"Robocop", 25}
	fmt.Println(a) // Robocop, power 25
	
	b := Robot{"Power", 50}
	fmt.Println(b) // Power, power 50
}
```

