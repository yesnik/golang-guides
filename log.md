# Log

A big benefit of logging your messages to the standard streams (stdout and stderr) is that our application and logging are decoupled. 
Our application itself isn't concerned with the routing or storage of the logs.

## Info log

Use `log.New()` to create a logger for writing information messages. 
This takes 3 parameters: 

- the destination to write the logs to (os.Stdout),
- a string prefix for message (INFO followed by a tab)
- flags to indicate what additional information to include (local date and time). Note that the flags are joined using the bitwise `OR` operator `|`.

```go
infoLog := log.New(os.Stdout, "INFO\t", log.Ldate|log.Ltime)

infoLog.Printf("Starting server on %s", *addr)
```

## Error log

Create a logger for writing error messages in the same way, but use `stderr` as the destination and use the `log.Llongfile` flag to include the relevant file name and line number.

```go
errorLog := log.New(os.Stderr, "ERROR\t", log.Ldate|log.Ltime|log.Llongfile)

errorLog.Fatal(err)
```

We can also force your logger to use UTC datetimes (instead of local ones) by adding the `log.LUTC` flag.

## Log to files

We could redirect the `stdout` and `stderr` streams to files when starting the application:

```
go run ./cmd/web >>./info.log 2>> ./error.log
```

Log to a file:

```go
f, err := os.OpenFile("./info.log", os.O_RDWR|os.O_CREATE, 0666)
if err != nil {
  log.Fatal(err)
}
defer f.Close()
infoLog2 := log.New(f, "INFO\t", log.Ldate|log.Ltime)
infoLog2.Printf("Hello")
```
