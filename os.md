# OS module

Package [os](https://pkg.go.dev/os) provides a platform-independent interface to operating system functionality.

## os.Args

`Args` hold the command-line arguments, starting with the program name.

*Example:* `go run myapp.go aa bb cc`

```go
fmt.Println(os.Args)
// [C:\Users\kenny\AppData\Local\Temp\go-build3300351132\b001\exe\average2.exe aa bb cc]
```

## os.FileMode

A `FileMode` type represents a file's mode and permission bits. 
The bits have the same definition on all systems, so that information about files can be moved from one system to another portably. 
Not all bits apply to all systems. 

```go
fmt.Println(os.FileMode(0400)) // -r--------
fmt.Println(os.FileMode(0600)) // -rw-------
fmt.Println(os.FileMode(0700)) // -rwx------
fmt.Println(os.FileMode(0070)) // ----rwx---
fmt.Println(os.FileMode(0007)) // -------rwx
fmt.Println(os.FileMode(0777)) // -rwxrwxrwx
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
