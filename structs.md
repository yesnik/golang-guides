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
