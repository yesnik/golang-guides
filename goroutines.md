# Goroutines

In Go, concurrent tasks are called **goroutines**. 
Other programming languages have a similar concept called threads, but goroutines require less computer memory than threads, and less time to start up and stop, meaning you can run more goroutines at once.

A goroutine is a lightweight thread managed by the Go runtime.

To start *another* goroutine, you use a `go` statement, which is just an ordinary function or method call with the `go` keyword in front of it.
Notice that we say *another* goroutine. The `main` function of every Go program is started using a *goroutine*, so every Go program runs at least one goroutine.

Goroutines allow for concurrency: pausing one task to work on others. 
And in some situations they allow parallelism: working on multiple tasks simultaneously!

```go
go f(x, y)
```

starts a new goroutine running

```go
f(x, y)
```

The evaluation of `f`, `x`, `y` happens in the current goroutine and the execution of `f` happens in the new goroutine.

Goroutines run in the same address space, so access to shared memory must be synchronized. 
The `sync` package provides useful primitives, although you won't need them much in Go as there are other primitives.

```go
import (
	"fmt"
	"time"
)

func a() {
	for i := 0; i < 100; i++ {
		fmt.Print("A")
	}
}
func b() {
	for i := 0; i < 100; i++ {
		fmt.Print("B")
	}
}

func main() {
	go a()
	go b()
	time.Sleep(time.Second)
	fmt.Println("end main()")
}
// Possible output: BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
// AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
// BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBend main()
```

The call to `time.Sleep` in the main goroutine gives more than enough time for both the a and b goroutines to finish running.

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

