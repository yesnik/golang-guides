# Strings

### strings.NewReplacer

```go
messageRaw := "Hexxo Worxd"
replacer := strings.NewReplacer("x", "l")

message := replacer.Replace(messageRaw) 
fmt.Println(message) // Hello World
```

### for ... range

```go
str := "Hello"
for i, charCode := range str {
	fmt.Println(i, charCode) // 0 72
	fmt.Printf("%c", charCode) // H
	fmt.Println(string(charCode)) // H
	fmt.Printf("%T", charCode) // int32
}
```

### strings.Join()

```go
str := strings.Join([]string{"hello", "dear", "world"}, "-");
fmt.Println(str) // hello-dear-world
```
