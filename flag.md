# Flag

### flag.String

```go
import (
  "flag"
  "fmt"
)

// Define a new command-line flag with the name 'addr', a default value of "127.0.0.1:4000"
// and some short help text explaining what the flag controls. The value of the
// flag will be stored in the addr variable at runtime.
addr := flag.String("addr", "127.0.0.1:4000", "HTTP network address")

// We must call this function after defining all flags and before using their values.
flag.Parse()

fmt.Printf("Hello, %s!\n", *addr)
```

### flag.Bool

```go
debug := flag.Bool("debug", false, "Is Debug enabled")
flag.Parse()
fmt.Println(*debug) // true
```

Command in the console:
```bash
go run main.go -debug=true
```

### flag.Int

```go
age := flag.Int("age", 0, "User's age")
flag.Parse()
fmt.Println(*age) // 18
```

Command in the console:
```bash
go run main.go -age=18
```

### Help Commands

It automatically generates a help message accessible via `-h` or `--help`: 

```bash
go run .\cmd\web -help

Usage of C:\Users\USR\AppData\Local\go-build\e9\e967ef59d4370fd91a6e181657b75f1a2ab74235b8a1784a1693b6dc9efa2a3e-d\web.exe:
  -addr string
        HTTP network address (default "127.0.0.1:4000")
```

### Positional arguments

Any arguments left after flags are parsed can be accessed via `flag.Args()`
