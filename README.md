# Go Documentation

## Installing Go

### Getting Started

If you are upgrading from an older version of Go you must first remove the existing version.

If you use *go build* in the source code directory, an executable file will be built alongside source code.

You can run *go install* to install the binary into your workspace's bin directory or *go clean* to remove it.

The *GOPATH* environment variable specifies the location of your workspace. If no *GOPATH* is set, it is assumed to be *$HOME/go* on Unix systems and *%USERPROFILE%\go* on Windows. If you want to use a custom location as your workspace, you can set the *GOPATH* environment variable.

## Learning Go

### A Tour of Go

### How to write Go codes

#### Introduction

#### Code organization

Go programmers typically keep all their Go code in a single workspace.

A workspace is a directory hierarchy with three directories at its root:

* src contains Go source files,
* pkg contains package objects, and
* bin contains executable commands.

Note that *GOPATH* must not be the same path as your Go installation.

The command *go env GOPATH* prints the effective current *GOPATH*; it prints the default location if the environment variable is unset.

For convenience, add the workspace's bin subdirectory to your PATH:

    $ export PATH=$PATH:$(go env GOPATH)/bin

A package's import path corresponds to its location inside a workspace or in a remote repository.

You can build and install that program with the go tool:

    go install github.com/ajtomato/Go.Documentation/hello
    hello is generated under $GOPATH/bin

OR

    cd $GOPATH/src/github.com/ajtomato/Go.Documentation/hello
    go install
    hello is generated under $GOPATH/bin

You can test the package compiles with the go tool. This won't produce an output file.

    go build github.com/ajtomato/Go.Documentation/stringutil
    No output file is generated.

OR

    cd $GOPATH/src/github.com/ajtomato/Go.Documentation/stringutil
    go build
    No output file is generated.

The first statement in a Go source file must be

    package name

where name is the package's default name for imports. (All files in a package must use the same name.)

Executable commands must always use *package main*.

#### Testing

You write a test by creating a file with a name ending in *_test.go* that contains functions named *TestXXX* with signature func (t *testing.T). The test framework runs each such function; if the function calls a failure function such as t.Error or t.Fail, the test is considered to have failed.

Run the test with go tool:

    go test github.com/ajtomato/Go.Documentation/stringutil

OR

    cd $GOPATH/src/github.com/ajtomato/Go.Documentation/stringutil
    go test

#### Remote packages

If you include the repository URL in the package's import path, go get will fetch, build, and install it automatically:

    $ go get github.com/golang/example/hello
    $ $GOPATH/bin/hello
    Hello, Go examples!

If the specified package is not present in a workspace, go get will place it inside the first workspace specified by *GOPATH*. (If the package does already exist, *go get* skips the remote fetch and behaves the same as *go install*.)

The *go get* command is able to locate and install the dependent package, too.

#### What's next
#### Getting help
