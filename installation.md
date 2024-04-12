# Installation

- https://go.dev/doc/install
- [Ubuntu](https://github.com/golang/go/wiki/Ubuntu)

## Simple program

Every Go program is made up of packages.
Programs start running in package `main`.

File `hello.go`:

```go
package main
  
import (
    "fmt"
    "time"
)

func main() {
    var age int = 18
    var name string = "Kenny"
    
    fmt.Println("Hello " + name)
    fmt.Println(age)
    fmt.Println("Current time: ", time.Now()) // 2020-10-25 00:45:14.986395317 +0500 +05 m=+0.000031018
}
```

### Commands

- `go run hello.go` - run program
- `go build hello.go` - build executable file `hello`
- `go env GOPATH` - show workspace path
- `go fmt hello.go` - format Go source code. Oh, *it uses tabs*, not spaces.

See other [console commands](console_commands.md)
