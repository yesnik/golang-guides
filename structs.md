# Structs

A *struct* is a collection of fields. 

Struct fields are accessed using a dot.

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 5
	fmt.Println(v.X) // 5
}
```

## Struct Literals

A struct literal is a newly allocated struct value.

You can list just a subset of fields in *any order* by using the `X: 123` syntax.

The `&` returns a pointer to the struct value.

```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{Y: 3}  // X:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{4, 5} // has type *Vertex
)

func main() {
	fmt.Println(v1, v2, v3, p) // {1 2} {0 3} {0 0} &{4 5}
}
```
