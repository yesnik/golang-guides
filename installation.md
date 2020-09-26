# Installation

- [Ubuntu](https://github.com/golang/go/wiki/Ubuntu)

## Commands

- `go run hello.go` - run program
- `go build hello.go` - build executable file hello
- `go env GOPATH` - show workspace path

## Simple program

File hello.go:

```go
package main
  
import (
    "fmt"
)

func main() {
    var age int = 18
    var name string = "Kenny"
    
    fmt.Println("Hello " + name)
    fmt.Println(age)
}
```
