# net

## net/http

Within `viewHandler`, we add data to the response by calling the `Write` method on the `ResponseWriter`. 
Method `Write` doesn't accept strings, but it does accept a slice of byte values, so we convert our "Hello, web!" string to a `[]byte`, then pass it to `Write`.

```go
import (
	"log"
	"net/http"
)

func home(writer http.ResponseWriter, request *http.Request) {
	if r.URL.Path != "/" {
		http.NotFound(w, r)
		return
	}

	message := []byte("Hello, web!")
	_, err := writer.Write(message)
	if err != nil {
		log.Fatal(err)
	}
}

func create(w http.ResponseWriter, r *http.Request) {
	if r.Method != "POST" {
		w.Header().Set("Allow", "POST")
		w.WriteHeader(405)
		w.Write([]byte("Method Not Allowed"))
		return
	}
	w.Write([]byte("Create..."))
}

func main() {
	// Use the http.NewServeMux() function to initialize a new servemux, then
	// register the home function as the handler for the "/" URL pattern.
	mux := http.NewServeMux()
	mux.HandleFunc("/", home)
	mux.HandleFunc("/create", create)

	log.Print("Starting server on 127.0.0.1:8000")
	err := http.ListenAndServe("127.0.0.1:8080", mux)
	log.Fatal(err)
}
```

Visit: http://127.0.0.1:8080/hello

- It's only possible to call `w.WriteHeader()` once per response, and after the status code has been written it can't be changed. If you try to call `w.WriteHeader()` a second time Go will log a warning message.
- If you don't call `w.WriteHeader()` explicitly, then the first call to `w.Write()` will automatically send a `200 OK` status code to the user. 
- `w.Header().Set()` method adds a new header to the response header map.

A slice of `byte` values won't show us anything meaningful if we print it directly, but if we do a type conversion from a slice of `byte` values to a `string`, we'll get readable text back.
So we end by converting the response body to a `string`, and printing it.

```go
// A []byte to a string
fmt.Println(string([]byte{72, 101, 108, 108, 111})) // Hello

// A string to a []byte
fmt.Println([]byte("Hello")) // [72 101 108 108 111]
```
