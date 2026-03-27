# net

## net/http

Within `viewHandler`, we add data to the response by calling the `Write` method on the `ResponseWriter`. 
Method `Write` doesn't accept strings, but it does accept a slice of byte values, so we convert our "Hello, web!" string to a `[]byte`, then pass it to `Write`.

```go
import (
	"log"
	"net/http"
)

func viewHandler(writer http.ResponseWriter, request *http.Request) {
	message := []byte("Hello, web!")
	_, err := writer.Write(message)
	if err != nil {
		log.Fatal(err)
	}
}

func main() {
	http.HandleFunc("/hello", viewHandler)
	err := http.ListenAndServe("127.0.0.1:8080", nil)
	log.Fatal(err)
}
```

Visit: http://127.0.0.1:8080/hello

A slice of `byte` values won't show us anything meaningful if we print it directly, but if we do a type conversion from a slice of `byte` values to a `string`, we'll get readable text back.
So we end by converting the response body to a `string`, and printing it.

```go
// A []byte to a string
fmt.Println(string([]byte{72, 101, 108, 108, 111})) // Hello

// A string to a []byte
fmt.Println([]byte("Hello")) // [72 101 108 108 111]
```
