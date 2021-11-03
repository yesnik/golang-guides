# Maps

A `map` maps keys to values.

The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added.

The `make` function returns a map of the given type, initialized and ready for use.

## Examples

### Example 1

```go
m := make(map[int]string)
m[1] = "NT"
m[5] = "NY"
fmt.Println(m[1]) // NT
fmt.Println(m[5]) // NY
```

### Example 2

```go
type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Ekb"] = Vertex{
		56.85, 60.61,
	}
	fmt.Println(m["Ekb"]) // {56.85 60.61}
}
```