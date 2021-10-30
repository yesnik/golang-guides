# Pointers

A *pointer* is a variable that stores the address of another variable.

- `&` operator generates a pointer of the variable.
- `*` - is the *dereference operator* / *indirection operator*. It shows the pointer's underlying value. 
  When it is used with the pointer variable, then it is known as *dereferencing a pointer*. 

```go
x := 5

p := &x // p is a pointer
fmt.Println(p) // 0xc000018030
fmt.Println(*p) // 5 - get value of `x` through a pointer

*p = 7 // set value to `x` through a pointer
fmt.Println(i) // 7
```
