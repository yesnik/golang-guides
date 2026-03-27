# net

## net/http

Within `viewHandler`, we add data to the response by calling the `Write` method on the `ResponseWriter`. 
Method `Write` doesn't accept strings, but it does accept a slice of byte values, so we convert our "Hello, web!" string to a `[]byte`, then pass it to Write.

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
