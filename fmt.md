# fmt package

[Docs](https://pkg.go.dev/fmt)

## fmt.Printf

```go
fmt.Printf("One-third: %0.4f\n", 1.0/3.0) // One-third: 0.3333
```

## fmt.Sprintf

It works just like `fmt.Printf`, except that it returns a formatted string instead of printing it.

```go
result := fmt.Sprintf("One-third: %0.2f\n", 1.0/3.0) 
fmt.Printf(result) // One-third: 0.33
```
