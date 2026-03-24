# OS module

Package [os](https://pkg.go.dev/os) provides a platform-independent interface to operating system functionality.

## os.Args

`Args` hold the command-line arguments, starting with the program name.

*Example:* `go run myapp.go aa bb cc`

```go
fmt.Println(os.Args)
// [C:\Users\kenny\AppData\Local\Temp\go-build3300351132\b001\exe\average2.exe aa bb cc]
```

## os.ReadDir

`os.ReadDir` reads the named directory and returns a slice of `os.DirEntry` values, sorted by filename. 

```go
import (
	"fmt"
	"os"
	"log"
)

func main() {
	files, err := os.ReadDir("my_directory")
	if err != nil {
		log.Fatal(err)
	}

	for _, file := range files {
		if file.IsDir() {
			fmt.Println("Directory:", file.Name())
		} else {
			fmt.Println("File:", file.Name())
		}
	}
}
```

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
