# Types

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
