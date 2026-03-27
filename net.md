# net

## net/http

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
