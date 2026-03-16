# Maps

- A `map` maps keys to values.
- Whereas arrays and slices can only use integers as indexes, a map can use (almost) any type for keys.
- All of a map's keys must be the same type, and all the values must be the same type.

```go
// Declare a map variable
var cities map[int]string
// Create the map
cities = make(map[int]string)

// Create a map and declare a variable to hold it
cities := make(map[int]string)

// Map literal
cities := map[int]string{11: "NY", 22: "NT"}
fmt.Println(cities) // map[11:NY 22:NT]
```

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

### Insert / update element

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

### Delete element

```go
delete(m, "age")
fmt.Println(m["age"]) // 0

// If key is not in the map, then elem is the zero value for the map's element type.
v, ok = m["age"]
fmt.Println("The value:", v, "Present?", ok) // The value: 0 Present? false
```

## Zero values apart from assigned values

Zero values, although useful, can sometimes make it difficult to tell whether a given key has been assigned the zero value, or if it has never been assigned.

To address situations like this, accessing a map key optionally returns a second, `Boolean` value. It will be `true` if the returned value has actually been assigned to the map, 
or `false` if the returned value just represents the default zero value.

```go
cities := map[string]int{"NY": 9, "MSK": 13}

fmt.Println(cities["MSK"]) // 13

var londonPopulation int
var ok bool
londonPopulation = cities["LO"]
fmt.Println(londonPopulation, ok) // 0 false
```

The Go maintainers refer to this as the *"comma ok idiom"*.

If you only want to test whether a value is present, you can have the value itself ignored by assigning it to the `_` blank identifier.

```go
_, ok := cities["LA"]
fmt.Println(ok) // false
```

## The `for ... range` loop handles maps in random order

The `for...range` loop processes map keys and values in a random order because a *map is an unordered collection of keys and values*. 
When you use a `for...range` loop with a map, you never know what order you’ll get the map's contents in! 
Sometimes that's fine, but if you need more consistent ordering, you'll need to write the code for that yourself.

```go
counts = map[string]int{"Kenny":11, "Jenny": 12, "Lenny": 5, "Some": 111, "Arny": 23}

for name, value := range counts {
	fmt.Printf("%s: %d\n", name, value)
}
```

Solution:

```go
counts = map[string]int{"Kenny":11, "Jenny": 12, "Lenny": 5, "Some": 111, "Arny": 23}

var names []string
for name := range counts {
	names = append(names, name)
}
fmt.Println(names) // [Kenny Jenny Lenny Some Arny]
sort.Strings(names)
for _, name := range names {
	fmt.Printf("%s: %d\n", name, counts[name])
}
```
