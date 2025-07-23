# Readers

The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data.

The `io.Reader` interface has a `Read` method:

```go
func (T) Read(b []byte) (n int, err error)
```

`Read` populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an `io.EOF` error when the stream ends.

This code creates a `strings.Reader` and consumes its output 4 bytes at a time:

```go
import (
	"fmt"
	"io"
	"strings"
)

func main() {
	reader := strings.NewReader("aabcdefghi")

	buffer := make([]byte, 4)
	for {
		n, err := reader.Read(buffer)
		
		fmt.Printf("n = %v, err = %v, buffer = %v\n", n, err, buffer)
		fmt.Printf("b[:n] = %q\n", buffer[:n])
		
		if err == io.EOF {
			break
		}
	}
}

/* Output: 
n = 4, err = <nil>, buffer = [97 97 98 99]
b[:n] = "aabc"
n = 4, err = <nil>, buffer = [100 101 102 103]
b[:n] = "defg"
n = 2, err = <nil>, buffer = [104 105 102 103]
b[:n] = "hi"
n = 0, err = EOF, buffer = [104 105 102 103]
b[:n] = ""
*/
```
