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
