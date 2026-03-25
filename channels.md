# Channels

**Channels** are a typed conduit through which you can send and receive values with the channel operator, `<-`.
It looks like an arrow pointing from the value we're sending to the channel we're sending it on.

```go
ch <- v    // Send v to channel ch. The data flows in the direction of the arrow.
```

We also use the `<-` operator to receive values from a channel. It kind of looks like we're pulling a value out of the channel:

```go
v := <-ch  // Receive from ch, and assign value to v.
```

Each channel only carries values of a particular type, so you might have one channel for `int` values, and another channel for values with a `struct` type. 

Like maps and slices, channels must be created before use:

```go
var myChannel chan int // Declare a variable to hold a channel
myChannel = make(chan int) // Actually create the channel

myChannel := make(chan int) // Create a channel and declare a variable at once
```

By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

The example code sums the numbers in a slice, distributing the work between two goroutines. Once both goroutines have completed their computation, it calculates the final result.

```go
import "fmt"

func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{1, 2, 3, 10, 20, 30}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y) // 60 6 66
}
```

### Example: hello

In the `main` function, we create the channel that we're going to pass to `hello` using the built-in `make` function. 
Then we call `hello()` as a new goroutine. 
Using a separate goroutine is important, because channels should only be used to communicate *between* goroutines.

```go
func hello(myChannel chan string) {
	myChannel <- "Hi"
}

func main() {
	myChannel := make(chan string)
	go hello(myChannel)
	receivedValue := <-myChannel
	fmt.Println(receivedValue)
}
```

## Buffered Channels

Channels can be buffered. Provide the buffer length as the second argument to make to initialize a buffered channel:

```go
ch := make(chan int, 100)
```

Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.

```go
func main() {
	ch := make(chan int, 2)
	ch <- 1
	ch <- 2
	fmt.Println(<-ch) // 1
	fmt.Println(<-ch) // 2
}
```

## Range and Close

A sender can close a channel to indicate that no more values will be sent. 
Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression:

```go
v, ok := <-ch
```

`ok` is `false` if there are no more values to receive and the channel is closed.

The loop for `i := range c` receives values from the channel repeatedly until it is closed.

**Note**: Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.

**Another note**: Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a range loop.

```go
func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 5)
	go fibonacci(cap(c), c)
	for i := range c {
		fmt.Println(i)
	}
}
```
