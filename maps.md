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

## Omit type from the literal's elements

If the top-level type is just a type name, you can omit it from the elements of the literal.

```go
type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{40.68433, -74.39967},
	// We can omit type name `Vertex`:
	"Google":    {37.42202, -122.08408},
}
```

## Mutating maps

```go
m := make(map[string]int)
```

Insert / update element:

```go
m["age"] = 16
fmt.Println(m["age"]) // 16

m["age"] = 21
fmt.Println(m["age"]) // 21
fmt.Println(m["some"]) // 0
```

Test that a key is present with a *two-value assignment*:

```go
// If key is in m, ok is true. 
v, ok := m["age"]
fmt.Println("The value:", v, "Present?", ok) // The value: 21 Present? true
```

Delete element:

```go
delete(m, "age")
fmt.Println(m["age"]) // 0

// If key is not in the map, then elem is the zero value for the map's element type.
v, ok = m["age"]
fmt.Println("The value:", v, "Present?", ok) // The value: 0 Present? false
```
