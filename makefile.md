# Makefile

You may not be familiar with `make`, but it’s been used to build programs on Unix systems since 1976.
It lets developers specify a set of operations that are necessary to build a program and the order in which the steps must be performed. 

Create a file called Makefile in the module's directory with the following contents:

```Makefile
.DEFAULT_GOAL := build
.PHONY:fmt vet build

fmt:
	go fmt ./...
vet: 
	go vet ./...
build: fmt vet 
	go build
```

- Each possible operation is called a *target*. 
- The `.DEFAULT_GOAL` defines which target is run when no target is specified.  In this case, the default is the `build` target.
- The word before the colon (:) is the name of the target. Any words after the target (like vet in the line `build: fmt vet`) are the other targets that must be run before the specified target runs.
- The tasks that are performed by the target are on the indented lines after the target.
- The `.PHONY` line keeps `make` from getting confused if a directory or file in your project has the same name as one of the listed targets.

Run make and you should see the following output:

```
go fmt ./...
go vet ./...
go build
```
