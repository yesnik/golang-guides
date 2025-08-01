# Hello World

## Example 1

1. File `hello.go`:
    ```go
    // Every Go file has to begin with a package clause
    package main

    // Go files must import	only the packages they reference
    import "fmt"
    
    //  Go	looks for a function named "main" to run first
    func main() {
        fmt.Println("Hello")
    }
    ```
2. Format the code: `go fmt hello.go`
3. Run the program: `go run hello.go`
4. Build binary file: `go build hello.go`
5. Run binary file: `./hello`

## Example 2

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
    The package `main` tells the Go compiler that the package should compile as an executable program instead of a shared library. 
    The `main()` function in the package `main` will be the entry point of our executable program. 
    When you build shared libraries, you will not have any `main` package and `main()` function in the package.
    
    This code uses [fmt](https://pkg.go.dev/fmt) package, which contains functions for formatting text, including printing to the console. 
3. Run code:
    ```bash
    go run .
    ```

## Example 3

1. Create file `greeting.go`:
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
