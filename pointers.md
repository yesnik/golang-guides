# Pointers

A *pointer* is a variable that stores the address of another variable.

- `&` operator generates a pointer of the variable.
- `*` - is the *dereference operator* / *indirection operator*. It shows the pointer's underlying value. 
  When it is used with the pointer variable, then it is known as *dereferencing a pointer*. 

```go
x := 5

p := &x // p is a pointer
fmt.Println(p) // 0xc000018030
fmt.Println(*p) // 5 - get value of `x` through a pointer

*p = 7 // set value to `x` through a pointer
fmt.Println(x) // 7
```

## Pointers to structs

Struct fields can be accessed through a *struct pointer*.

To access the field `Y` of a struct we could write `(*p).Y`. But this is cumbersome, so the language allows to write `p.X`, without the explicit *dereference*.

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
  
	p.Y = 5 // also we could write here: `(*p).Y = 5`
	fmt.Println(v) // {1 5}
}
```

## Methods and pointer indirection

Functions with a pointer argument must take a pointer:

```go
var v Vertex
ScaleFunc(v, 5)  // Compile error!
ScaleFunc(&v, 5) // OK
```

While methods with pointer receivers take either a value or a pointer as the receiver when they are called:

```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

For the statement `v.Scale(5)`, even though `v` is a value and not a pointer, the method with the pointer receiver is called automatically. That is, as a convenience, Go interprets the statement `v.Scale(5)` as `(&v).Scale(5)` since the Scale method has a pointer receiver.

```go
import "fmt"

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{2, 3}
	v.Scale(2)
	fmt.Println(v) // {4 6}
	
	ScaleFunc(&v, 10)
	fmt.Println(v) // {40 60}

	p := &Vertex{2, 3}
	p.Scale(2)
	fmt.Println(p) // &{4 6}
	ScaleFunc(p, 10)
	fmt.Println(p) // &{40 60}
}
```
