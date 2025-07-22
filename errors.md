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
