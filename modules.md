# Modules

In version 1.13, the authors of Go added a new way of managing the libraries a Go project depends on, called [Go modules](https://go.dev/ref/mod).

Modules are how Go manages dependencies.

A module is a collection of packages that are released, versioned, and distributed together. Modules may be downloaded directly from version control repositories or from module proxy servers.

A module is identified by a module path, which is declared in a `go.mod` file, together with information about the module’s dependencies. 
The module root directory is the directory that contains the `go.mod` file. 
The main module is the module containing the directory where the go command is invoked.

