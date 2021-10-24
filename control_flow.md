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
