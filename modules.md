# Modules

In version 1.13, the authors of Go added a new way of managing the libraries a Go project depends on, called [Go modules](https://go.dev/ref/mod).

Go code is grouped into packages, and packages are grouped into modules. 
Your module specifies dependencies needed to run your code, including the Go version and the set of other modules it requires.

A module is a collection of packages that are released, versioned, and distributed together. 
Modules may be downloaded directly from version control repositories or from module proxy servers.

As you add or improve functionality in your module, you publish new versions of the module. 
Developers writing code that calls functions in your module can import the module's updated packages and test with the new version before putting it into production use.

A module is identified by a module path, which is declared in a `go.mod` file, together with information about the module’s dependencies. 
The module root directory is the directory that contains the `go.mod` file. 
The `main` module is the module containing the directory where the go command is invoked.

You should include the following files in the repository:

- `go.mod`: This is your project's manifest. It defines the module path and lists the specific versions of dependencies your code requires. Without it, other developers or CI/CD pipelines won't know which versions of libraries to use, which can lead to broken builds if a dependency releases a breaking change.
- `go.sum`: This file contains cryptographic checksums for your dependencies. It acts as a security net, ensuring that the source code of a dependency hasn't been tampered with or modified since you first added it. If someone clones your repo and the downloaded dependency doesn't match the checksum in `go.sum`, Go will throw an error.

## Create a module

To create a module run the `go mod init` command in the empty directory of your module:

```bash
mkdir greetings
cd greetings
go mod init example.com/greetings
```

The `go mod init` command creates a `go.mod` file to track your code's dependencies. 
So far, the file includes only the name of your module and the Go version your code supports. 
But as you add dependencies, the `go.mod` file will list the versions your code depends on. 
This keeps builds reproducible and gives you direct control over which module versions to use.


