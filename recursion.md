# Recursion

Here's a recursive `count` function that counts from a starting number up to an ending number.

```go
func count(start int, end int) {
	fmt.Printf("count(%d, %d) called\n", start, end)
	fmt.Println(start)
	if start < end {
		count(start + 1, end)
	}
	fmt.Printf("Returning from count(%d, %d)\n", start, end)
}

func main() {
	count(1, 3)
}
/*
count(1, 3) called
1
count(2, 3) called
2
count(3, 3) called
3
Returning from count(3, 3)
Returning from count(2, 3)
Returning from count(1, 3)
*/
```

## Print names of files and directories

Recursive functions can be tricky to write, and they often consume more computing resources than nonrecursive solutions. 
But sometimes, recursive functions offer solutions to problems that would be very difficult to solve using other means.

```go
import (
	"fmt"
	"log"
	"os"
	"path/filepath"
)

func scanDirectory(path string) error {
	fmt.Println(path)
	files, err := os.ReadDir(path)
	if err != nil {
		return err
	}

	for _, file := range files {
		filePath := filepath.Join(path, file.Name())
		if file.IsDir() {
			err := scanDirectory(filePath)
			if err != nil {
				return err
			}
		} else {
			fmt.Println(filePath)
		}
	}
	return nil
}

func main() {
	err := scanDirectory("my_directory")
	if err != nil {
		log.Fatal(err)
	}
}
```
