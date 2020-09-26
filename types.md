# Types

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
