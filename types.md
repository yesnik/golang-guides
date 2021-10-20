# Types

## Built-in 17 Basic Types in Go

- boolean type - `bool`
- 11 built-in integer numeric types:
    - `int8`, `uint8` (alias `byte`)
    - `int16`, `uint16` 
    - `int32`, `uint32` (alias `rune`) 
    - `int64`, `uint64`
    - `int`, `uint`, `uintptr`.
- 2 built-in floating-point numeric types: `float32`, `float64`.
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

The zero value of a type can be viewed as the default value of the type:

- `boolean` type - false
- `numeric` type - 0 (different numeric types may have different sizes in memory)
- `string` type - empty string

Examples:

```go
// Way 1
var age int = 18

// Way 2
age := 18
```

## Numbers

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
