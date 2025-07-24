# Goroutines

A goroutine is a lightweight thread managed by the Go runtime.

```go
go f(x, y, z)
```

starts a new goroutine running

```go
f(x, y, z)
```

The evaluation of `f`, `x`, `y`, `z` happens in the current goroutine and the execution of `f` happens in the new goroutine.

Goroutines run in the same address space, so access to shared memory must be synchronized. 
The `sync` package provides useful primitives, although you won't need them much in Go as there are other primitives.

```go
import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 3; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Print(s)
	}
}

func main() {
	go say("1")
	say("2")
}
// Possible output: 21122
```

## Select

The `select` statement lets a goroutine wait on multiple communication operations.

A `select` blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

The `default` case in a `select` is run if no other case is ready.

```go
import (
	"fmt"
	"time"
)

func main() {
	c1 := make(chan string)
	c2 := make(chan string)

	// Goroutine 1: Sends "one" to c1 after 1 second
	go func() {
		time.Sleep(time.Second * 1)
		c1 <- "1"
	}()

	// Goroutine 2: Sends "two" to c2 after 2 seconds
	go func() {
		time.Sleep(time.Second * 2)
		c2 <- "2"
	}()

	// The select statement waits for data from either c1 or c2
	for i := 0; i < 4; i++ {
		select {
		case msg1 := <-c1:
			fmt.Print(msg1) // received one
		case msg2 := <-c2:
			fmt.Print(msg2) // received two
		default:
			fmt.Printf("0")
			time.Sleep(time.Second)
		}
	}
	// Possible output: 0012
}
```

