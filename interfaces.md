# Interfaces

An interface type is defined as a set of method signatures.

A type implements an interface *by implementing its methods*. There is *no explicit declaration of intent*, no "implements" keyword.

Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

```go
type Questionable interface {
	AddQuestionMark() string
}

type Word struct {
	W string
}

// This method means type Word implements the interface Questionable,
// but we don't need to explicitly declare that it does so.
func (w Word) AddQuestionMark() string {
	return w.W + "?"
}

func main() {
  // A value of interface type can hold any value that implements those methods
	var leo Questionable = Word{"leo"}
	fmt.Println(leo.AddQuestionMark()) // leo?
}
```
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
