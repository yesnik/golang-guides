# Math package

Docs: https://pkg.go.dev/math

## Methods

- `math.Abs(-5) // 5`
- `math.Ceil(4.01) // 5`
- `math.Floor(5.99) // 5`
- `math.Pow(2, 3) // 8`
- `math.Round(5.4) // 5`
- `math.Sqrt(81) // 9`

## `math/rand`

```go
import (
	"fmt"
	"math/rand"
)

func main() {
	target := rand.Intn(3) // It will generate number from 0 to 2
	fmt.Println(target)
}
```
