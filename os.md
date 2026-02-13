# OS module

Package [os](https://pkg.go.dev/os) provides a platform-independent interface to operating system functionality.

## os.Stat

### fileInfo.Size()

It returns size of a file in bytes.

```go
import (
	"fmt"
	"log"
	"os"
)

func main() {
	fileInfo, err := os.Stat("file.txt")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(fileInfo.Size()) // Size of a file in bytes
}
```
