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

## Skip index or value

We can skip the index or value by assigning to `_`.

If we only want the index, we can omit the second variable.

```go
func main() {
	nums := []int{11, 22, 33, 44}
	
	for i := range nums {
		fmt.Println(i) // 0 1 2 3
	}
	
	for _, v := range nums {
		fmt.Println(v) // 11 22 33 44
	}
}
```
