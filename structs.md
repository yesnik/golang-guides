# Structs

*Struct* (short for "structure") in Golang is a user-defined type that allows to group/combine items of possibly different types into a single type. 
Any real-world entity which has some set of properties/fields can be represented as a struct. 

Struct fields are accessed using a dot.

We can define a variable as struct:

```go
var myStruct struct {
	name string
	population int
	active bool
}
myStruct = struct {
	name string
	population int
	active bool
} {"New York", 9, true}

fmt.Printf("%#v\n", myStruct)
// struct { name string; population int; active bool }{name:"Moscow", population:6, active:true}
```

Also we can define a custom type as struct. 
Type definitions allow us to create types of our own. They let us create a new defined type that's based on an *underlying type*.

```go
type Address struct {
	city       string
	street     string
	homeNumber int
}

func main() {
	a := Address{city: "Moscow", street: "Lenina", HomeNumber: 1}
	fmt.Println(a) // {Moscow Lenina 1}

	// We can initialize a variable of a struct type using a struct literal
	b := Address{"NY", "Yellow", 2}
	fmt.Println(b)            // {NY Yellow 2}
	fmt.Println(b.city)       // NY
	fmt.Println(b.street)     // Yellow
	fmt.Println(b.HomeNumber) // 2
}
```

- The `type` keyword introduces a new type. 
- It is followed by the name of the type (`Address`) and the keyword `struct` to illustrate that we're defining a struct. 
- The struct contains a list of various *fields* inside the curly braces.
- Each field has a name and a type.

We can also make struct compact by combining the various fields of the same type:

```go
type Address struct {
	city, street string
	HomeNumber   int
}
```

## Pointers to struct

To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`. However, the language permits us instead to write just `p.X`:

```go
type Vertex struct {
	X, Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	// Way 1
	(*p).X = 11
	// Way 2
	p.X = 11
	fmt.Println(v) // Output: {11 2}
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

## Embedding structs

An inner struct that is stored within an outer struct using an anonymous field is said to be embedded within the outer struct. 
Fields for an embedded struct are promoted to the outer struct, meaning you can access them as if they belong to the outer struct.

So now that the `Address` struct type is embedded within the `Employee` struct types, you don't have to write out `employee.Address.City`
to get at the `City` field; you can just write `employee.City`.

```go
type Address struct {
	Street string
	City   string
	State  string
	PostalCode string
}

type Employee struct {
	Name   string
	Salary float64
	Address // <-- Embedded struct here. We omitted attribute name
}

address := magazine.Address{
	Street: "333 Saint St",
	City: "New York",
	State: "NY",
	PostalCode: "123123",
}
subscriber := magazine.Subscriber{Name: "John Doe"}
subscriber.Address = address

fmt.Println(subscriber.Address.City) // New York
fmt.Println(subscriber.City) // New York
```
