# Types

## Built-in 17 Basic Types in Go

- boolean type - `bool`
- 11 built-in integer numeric types:
    - `int8`, `uint8` (alias `byte`)
    - `int16`, `uint16` 
    - `int32`, `uint32` (alias `rune`, it represents a Unicode code point)
        ```go
		fmt.Println('A') // 65 
	    fmt.Println('a') // 97
        fmt.Println(reflect.TypeOf('A')) // int32
        ```
    - `int64`, `uint64`
    - `int`, `uint`, `uintptr`.
- 2 built-in floating-point numeric types: `float32`, `float64`:
    ```go
    fmt.Println(reflect.TypeOf(1.5)) // float64
    ```
- 2 built-in complex numeric types: `complex64`, `complex128`.
- built-in string type: `string`.

### Notes

- `u` in `uint8` means unsigned type. Values of unsigned types are always non-negative. 
- The number in the name of a type means how many binary bits a value of the type will occupy in memory at run time:
    - Value of the `uint8` occupies 8 bits in memory.
    - The largest `uint8` value is 255 (2^8-1)
    - The smallest `int8` value is -128 (-2^7), largest `int8` value is 127 (2^7-1), 
- We measure the size of a value based on the number of bytes it occupies in memory. One `byte` contains 8 bits. So the size of the `uint32` type is 4 bytes.

### Zero Values

Variables declared without an explicit initial value are given their *zero value*.

- `false` - for the `boolean` type
- `0` - for numeric types (different numeric types may have different sizes in memory)
- `""` (empty string) - for `string` type

```go
func main() {
	var a int
	var b float64
	var active bool
	var name string
	fmt.Printf("%v %v %v %q\n", a, b, active, name) // Output: 0 0 false ""
}
```

## Numbers

When you need an integer value you should use `in`t unless you have a specific reason to use a sized or unsigned integer type.

### Signed

- `int`  
- `int8` (-128 - 127)
- `int16` (-32768 - 32767)
- `int32`
- `int64`

### Unsigned

- `uint`
- `uint8` / `byte` (0 - 255) 
- `uint16` (0 - 65535)
- `uint32` 
- `uint64`

### Float

- `float32`
- `float64`

Examples:

```go
var speed = 65.3
fmt.Printf("%T", speed) // float64
```

## Arrays

```go
var nums [5]int
fmt.Println(nums) // [0 0 0 0 0]

nums[0] = 11
nums[3] = 33
fmt.Println(nums) // [11 0 0 33 0]

arr := [5]int{5, 4, 3, 2, 1}
fmt.Println(arr) // [5 4 3 2 1]
```

### append

```go
arr := []int{1, 2, 3}
var arr_new []int = append(arr, 4)
fmt.Println(arr_new)
```

## Maps

```go
figures := make(map[string]int)

figures["rectangle"] = 2
figures["circle"] = 5

fmt.Println(figures) // map[circle:5 rectangle:2]

fmt.Println(figures["circle"]) // 5

delete(figures, "circle")
fmt.Println(figures)
```

## Types have default values

```go
var age int
fmt.Println(age) // 0

var enabled bool
fmt.Println(enabled) // fales
```

## Print type of the variable

```go
age := 18
fmt.Printf("Type %T, value %v", age, age) // Type int, value 18
```

## Type conversions

Unlike in C, in Go assignment between items of different type requires an explicit conversion. 

```go
var a, b int = 3, 4
var z float64 = math.Sqrt(float64(a*a + b*b))

var c int = z // Error: cannot use z (variable of type float64) as int value in variable declaration
var c int = int(z) // No error

fmt.Println(a, b, c) // Output: 3 4 5
```

## Type inference

When the right hand side of the declaration is typed, the new variable is of that same type:

```go
a := 5
fmt.Printf("%v is of type %T\n", a, a) // Output: 5 is of type int

b := 5.3
fmt.Printf("%v is of type %T\n", b, b) // Output: 5.3 is of type float64

c := 1234567890123456789
fmt.Printf("%v is of type %T\n", c, c) // Output: 1234567890123456789 is of type int

d := "John"
fmt.Printf("%v is of type %T\n", d, d) // Output: John is of type string
```
