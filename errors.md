# Errors

Go programs express error state with error values.

The error type is a built-in interface similar to `fmt.Stringer`:

```go
type error interface {
    Error() string
}
```

Functions often return an error value, and calling code should handle errors by testing whether the error equals `nil`.

```go
import (
	"fmt"
	"strconv"
)

func main() {
	i, err := strconv.Atoi("123")
	if err != nil {
		fmt.Printf("couldn't convert number: %v\n", err)
		return
	}
	fmt.Println("Converted integer:", i) // 123
}
```

## Error value to a function in the `fmt` or `log` packages

Functions in fmt and log have been written to check whether the values passed to them have Error methods, and print the return value of Error if they do.

```go
import (
	"fmt"
	"errors"
    "log"
)

err := errors.New("Value can't be negative")
fmt.Println(err.Error()) // Value can't be negative
fmt.Println(err) // Value can't be negative

// Below method will exit the program with the status code 1
log.Fatal(err) // 2026/02/20 12:57:52 Value can't be negative
```
