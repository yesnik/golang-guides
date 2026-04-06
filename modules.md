# Modules

In version 1.13, the authors of Go added a new way of managing the libraries a Go project depends on, called [Go modules](https://go.dev/ref/mod).

Modules are how Go manages dependencies.

A module is a collection of packages that are released, versioned, and distributed together. Modules may be downloaded directly from version control repositories or from module proxy servers.

A module is identified by a module path, which is declared in a `go.mod` file, together with information about the module’s dependencies. 
The module root directory is the directory that contains the `go.mod` file. 
The main module is the module containing the directory where the go command is invoked.

You should include the following files in the repository:

- `go.mod`: This is your project's manifest. It defines the module path and lists the specific versions of dependencies your code requires. Without it, other developers or CI/CD pipelines won't know which versions of libraries to use, which can lead to broken builds if a dependency releases a breaking change.
- `go.sum`: This file contains cryptographic checksums for your dependencies. It acts as a security net, ensuring that the source code of a dependency hasn't been tampered with or modified since you first added it. If someone clones your repo and the downloaded dependency doesn't match the checksum in `go.sum`, Go will throw an error.
