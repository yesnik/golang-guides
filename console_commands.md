# Console Commands

- `go mod init github.com/username/mymodule` - init module, file `go.mod` will be created. Using a fully qualified module path ensures your module can be imported and versioned correctly.
- `go get .` - get dependencies for code in the current directory
- `go get github.com/gin-gonic/gin` - install framework Gin. File `go.mod` - updated, `go.sum` - created. Files will be copied to `C:\Users\kenny\go\pkg\mod\github.com\gin-gonic\gin@v1.11.0`
- `go mod edit -replace example.com/greetings=../greetings` - The command specifies that `example.com/greetings` should be replaced with `../greetings` for the purpose of locating the dependency.
   After you run the command, the `go.mod` file in the hello directory should include a replace directive: `replace example.com/greetings => ../greetings`
- `go mod tidy` - it ensures that the `go.mod` file matches the source code in the module.
    It adds any missing module requirements necessary to build the current module’s packages and dependencies, and it removes requirements on modules that don’t provide any relevant packages.
    It also adds any missing entries to `go.sum` and removes unnecessary entries.
- `go run hello.go` - run program
- `go run .` - run the package in the current directory
- `go build hello.go` - build executable file `hello`
- `go env GOPATH` - show workspace path
- `go install` - command without the `@version` syntax is for installing from the current directory
- `go install module@version` - download and install from a remote repository. 
- `go fmt hello.go` - format Go source code. Oh, *it uses tabs*, not spaces.
- `go doc strconv` - show docs for a package `strconv`
- `go doc strconv ParseFloat` - show docs for the `ParseFloat` function in the package `strconv`
- `go test -v` - run tests
