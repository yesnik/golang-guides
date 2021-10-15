# Hello World

## Example 1

1. Init module: 
    ```bash
    go mod init example/hello
    ```
    This command creates a `go.mod` file to track your code's dependencies.

2. Create file `hello.go`:
    ```go
    package main

    import "fmt"

    func main() {
        fmt.Println("Hello, World!")
    }
    ```
    This code uses [fmt](https://pkg.go.dev/fmt) package, which contains functions for formatting text, including printing to the console. 
3. Run code:
    ```bash
    go run .
    ```

## Example 2

1. Create file `greeting.go`
    ```go
    package main

    import "fmt"

    import "rsc.io/quote"

    func main() {
        fmt.Println(quote.Go())
    }
    ```
    We use here external package [rsc.io/quote](https://pkg.go.dev/rsc.io/quote).
2. Add new module requirements and sums. Go will add the quote module as a requirement, as well as a go.sum file for use in authenticating the module.
    ```bash
    go mod tidy
    ```
3. Run program
    ```bash
    go run .\greeting.go
    # Don't communicate by sharing memory, share memory by communicating.
    ```
