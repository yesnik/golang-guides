# Range

The `range` form of the `for` loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration: 

- first - the index
- second - copy of the element at that index

```go
nums := []int{2, 4, 6}

for i, v := range nums {
  fmt.Printf("Index: %d - value: %d\n", i, v)
}
/*
Index: 0 - value: 2
Index: 1 - value: 4
Index: 2 - value: 6
*/
```
