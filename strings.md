# Strings

```go
import (
	"fmt"
	"strings"
)

func main() {
	messageRaw := "Hexxo Worxd"
	replacer := strings.NewReplacer("x", "l")

	message := replacer.Replace(messageRaw) 
	fmt.Println(message) // Hello World
}
```
