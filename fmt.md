# fmt package

[Docs](https://pkg.go.dev/fmt)

## fmt.Errorf

It formats according to a format specifier and returns the string as a value that satisfies error.

```go
err := fmt.Errorf("Value %.2f is invalid", -2.33)
fmt.Println(err.Error()) // Value -2.33 is invalid
fmt.Println(err) // Value -2.33 is invalid
```

## fmt.Printf

```go
fmt.Printf("One-third: %0.4f\n", 1.0/3.0) // One-third: 0.3333
```

The letter following the percent sign `%` indicates which verb to use. The most common verbs are:

```go
fmt.Printf("A float: %f\n", 12.34) // A float: 12.340000
fmt.Printf("An int: %d\n", 5) // An int: 5
fmt.Printf("A string: %s\n", "hello") // A string: hello
fmt.Printf("A boolean: %t\n", true) // A boolean: true
fmt.Printf("Values: %v %v %v\n", "Hey", 111, 2.5) // Values: Hey 111 2.5
fmt.Printf("Types: %T %T %T\n", 2.3, "hey", false) // Types: float64 string bool
fmt.Printf("Percent sign: 100%%\n") // Percent sign: 100%

fmt.Printf("%12s | %2d\n", "Book", 95) //         Book | 95
```

The `%#v` verb used by the `Printf` and `Sprintf` functions, which formats values as they'd appear in Go code:

```go
numbers := [3]int{1, 2}
fmt.Printf("%#v\n", numbers) // [3]int{1, 2, 0}

fmt.Printf("Values: %#v %#v %#v\n", "\n", "\t", "") // Values: "\n" "\t" ""
```

## fmt.Println

Functions in the `fmt` package know how to handle arrays.

```go
numbers := [3]int{1, 2}
fmt.Println(numbers) // [1 2 0]
```

## fmt.Sprintf

It works just like `fmt.Printf`, except that it returns a formatted string instead of printing it.

```go
result := fmt.Sprintf("One-third: %0.2f\n", 1.0/3.0) 
fmt.Printf(result) // One-third: 0.33
```
