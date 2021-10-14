# Hello World program

1. Create `go.mod` file. When your code imports packages contained in other modules, 
you manage those dependencies through your code's own module. That module is defined by a `go.mod` file 
that tracks the modules that provide those packages. 
    ```bash
    go mod init example/hello
    ```
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
