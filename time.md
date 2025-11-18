# Time

```go
import (
	"fmt"
	"time"
)

func main() {
	var now time.Time = time.Now()
	var year int = now.Year()
	fmt.Println(year)          // 2025
	fmt.Println(now.Day())     // 18
	fmt.Println(now.Month())   // November
	fmt.Println(now.Date())    // 2025 November 18
	fmt.Println(now.Weekday()) // Tuesday
}
```
