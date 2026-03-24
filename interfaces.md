# Interfaces

We don't care whether we have a `Pen` or a `Pencil`, we just need something with a `Draw` method. 
We don't care whether we have a `Car` or a `Boat`, you just need something with a `Steer` method.
That's what Go *interfaces* accomplish. 
They let us define variables and function parameters that will hold *any* type, as long as that type defines certain methods.

An interface type is defined as a set of method signatures.

Interface types don't describe what a value is: they don't say what its underlying type is, or how its data is stored. 
They only describe what a value can do: what methods it has.

```go
type myInterface interface {
	method()
    methodWithParams(int)
    methodWithReturn() string
}
```

Any type that has all the methods listed in an interface definition is said to *satisfy* that interface. 
A type that satisfies an interface can be assigned to any variable or function parameter that uses that interface as its type.

A type can have methods in addition to those listed in the interface, but it mustn't be missing any, or it doesn't satisfy that interface.

A type implements an interface *by implementing its methods*. There is *no explicit declaration of intent*, no "implements" keyword.

A type can satisfy multiple interfaces, and an interface can have multiple types that satisfy it.

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

## Call methods defined as part of the interface

Once you assign a value to a variable (or method parameter) with an `interface` type, you *can only call methods* that are specified by the interface on it.

```go
type Whistle string

func (w Whistle) MakeSound() {
	fmt.Println("Tweet!")
}

func (w Whistle) MakeSuperSound() {
	fmt.Println("Super Tweet!")
}

type NoiseMaker interface {
	MakeSound()
}

func play(n NoiseMaker) {
	n.MakeSound()
    n.MakeSuperSound() // Error: n.MakeSuperSound undefined (type NoiseMaker has no field or method MakeSuperSound)
}

func main() {
	var toy NoiseMaker
	toy = Whistle("Toyco Canary")
	play(toy)
}
```
When we have a variable of an interface type, the only methods we can be sure it has are the methods that are defined in the interface. 
And so those are the only methods Go allows us to call.

**Note:** It's just fine to assign a type that has other methods to a variable with an interface type. 
As long as you don't actually call those other methods, everything will work.

## Methods with pointer receivers

If a type declares methods with pointer receivers, then we'll only be able to use pointers to that type when assigning to interface variables.
The `toggle` method on the `Switch` type below has to use a pointer receiver so it can modify the receiver.

```go
type Switch string
func (s *Switch) toggle() {
	if *s == "on" {
		*s = "off"
	} else {
		*s = "on"
	}
	fmt.Println(*s)
}

type Toggleable interface {
	toggle()
}

func main() {
	s := Switch("off")
	var t Toggleable = s
	// Error: cannot use s (variable of string type Switch) as Toggleable value in variable declaration: Switch does not implement Toggleable (method toggle has pointer receiver)
	t.toggle()
	t.toggle()
}
```

When Go decides whether a value satisfies an interface, pointer methods aren't included for direct values. 
But they are included for pointers. So the solution is to assign a pointer to a `Switch` to the `Toggleable` variable, instead of a direct `Switch` value:

```go
var t Toggleable = &s
```

---

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

An empty interface may hold values of any type. 
The empty interface doesn't have any methods that are required to satisfy it, and so every type satisfies it.

Empty interfaces are used by code that handles values of unknown type. 
For example, `fmt.Print` takes any number of arguments of type `interface{}`.

If we declare a function that accepts a parameter with the empty interface as its type, then we can pass it values of any type as an argument:

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

### Type assertion example

When we have a value of a concrete type assigned to a variable with an interface type, a type assertion lets us get the concrete type back. 
It's kind of like a type conversion.

```go
type TapePlayer struct {
	Batteries string
}
func (t TapePlayer) Play(song string) {
	fmt.Println("Playing", song)
}
func (t TapePlayer) Stop() {
	fmt.Println("Stopped!")
}

type TapeRecorder struct {
	Microphones int
}
func (t TapeRecorder) Play(song string) {
	fmt.Println("Playing", song)
}
func (t TapeRecorder) Record() {
	fmt.Println("Recording")
}
func (t TapeRecorder) Stop() {
	fmt.Println("Stopped!")
}

type Player interface {
	Play(string)
	Stop()
}

func playList(device Player, songs []string) {
	for _, song := range songs {
		device.Play(song)
	}
	device.Stop()
}

func main() {
	var player Player = TapeRecorder{}
	mixtape := []string{"Jessie's Girl", "Whip It", "9 to 5"}
	playList(player, mixtape)

	var tapeRecorder TapeRecorder = player.(TapeRecorder) // <-- Type assertion
	tapeRecorder.Record() // Recording
}
```

In plain language, the type assertion above says something like "I know this variable uses the interface type `Player`, but I'm pretty sure this `Player` is actually a `TapeRecorder`."

### Attempt to do a type assertion

We're creating a `TryVehicle` method that calls all the methods from the `Vehicle` interface. 
Then, we try to do a type assertion to get a concrete `Truck` value. 
If successful, we call `LoadCargo` on the `Truck` value.

```go
type Truck string

func (t Truck) Accelerate() {
	fmt.Println("Speeding up")
}
func (t Truck) Brake() {
	fmt.Println("Stopping")
}
func (t Truck) LoadCargo(cargo string) {
	fmt.Println("Loading", cargo)
}

type Vehicle interface {
	Accelerate()
	Brake()
}

func TryVehicle(vehicle Vehicle) {
	vehicle.Accelerate()
	vehicle.Brake()
	truck, ok := vehicle.(Truck) // <-- attempt a type assertion to get a concrete Truck value
	if ok {
		truck.LoadCargo("food")
	}
}

func main() {
	TryVehicle(Truck("Toyota Tundra"))
}
```

**Note**: This is another place Go follows the "comma ok idiom" that we can also see when accessing maps.

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

## Implement an `error` interface

```go
type OverheatError float64
func (o OverheatError) Error() string {
	return fmt.Sprintf("Overheating by %0.2f degrees!", o)
}

func checkTemperature(actual float64, safe float64) error {
	excess := actual - safe
	if excess > 0 {
		return OverheatError(excess)
	}
	return nil
}

func main() {
	var err error = checkTemperature(120.0, 100.0)
	if err != nil {
		log.Fatal(err) // 2026/03/20 20:00:15 Overheating by 20.00 degrees!
	}
}
```

The `error` type is a *predeclared identifier*, like `int` or `string`. And so, like other predeclared identifiers, it's not part of any package. 
It's part of the "universe block", meaning it's available everywhere, regardless of what package you're in.
