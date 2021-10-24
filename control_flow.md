# Control Flow Statements

## for loop

Go has **only one** looping construct, the `for` loop. The basic for loop has three components separated by semicolons:

- the *init* statement: executed before the first iteration
- the *condition* expression: evaluated before every iteration
- the *post* statement: executed at the end of every iteration

```go
sum := 0
for i := 1; i < 3; i++ {
  sum += i
}
fmt.Println(sum)
```

The init and post statements are optional:

```go
sum := 0
for ; sum < 3; {
  sum += 1
}
fmt.Println(sum)
```

C's `while` is spelled `for` in Go.

```go
sum := 1
for sum < 3 {
  sum += 1
}
fmt.Println(sum)
```

## if / else

```go
t := 95
if t > 90 {
  fmt.Println("Turn off")
}
```

### If with a short statement

Variables declared by the statement are only in scope until the end of the if. Also they are available inside any of the else blocks.

```go
t := 14
if base := "Turn "; t < 15 {
  fmt.Println(base + "on")
} else {
  fmt.Println(base + "off")
}
```

## switch

Go only runs the selected case, not all the cases that follow. 
The `break` statement that is needed at the end of each case in PHP, Java, Javascript is provided automatically in Go.
Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

```go
switch arrow := "S"; arrow {
case "N":
  fmt.Println("North")
case "S":
  fmt.Println("South")
default:
  fmt.Printf("Unknown arrow: %s.\n", arrow)
}
```

### switch without condition

Switch without a condition is the same as switch true.
This construct can be a clean way to write long if-then-else chains.

```go
t, _ := time.Parse("15:04:05", "20:30:01")
fmt.Println(t) // 0000-01-01 20:30:01 +0000 UTC
switch {
case t.Hour() < 12:
  fmt.Println("Good morning!")
case t.Hour() < 17:
  fmt.Println("Good afternoon.")
default:
  fmt.Println("Good evening.")
}
```

## Defer

A `defer` statement defers the execution of a function until the surrounding function returns.
The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```go
func main() {
    defer fmt.Println("world")

    fmt.Println("hello")
}
/*
hello
world
*/
```

Deferred function calls are **pushed onto a stack**. When a function returns, its deferred calls are executed in last-in-first-out order.

```go
func main() {
    for i := 1; i <= 3; i++ {
        defer fmt.Println(i)
    }
}
```
