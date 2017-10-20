# Go Documentation

## Installing Go

### Getting Started

If you are upgrading from an older version of Go you must first remove the existing version.

If you use *go build* in the source code directory, an executable file will be built alongside source code.

You can run *go install* to install the binary into your workspace's bin directory or *go clean* to remove it.

The *GOPATH* environment variable specifies the location of your workspace. If no *GOPATH* is set, it is assumed to be *$HOME/go* on Unix systems and *%USERPROFILE%\go* on Windows. If you want to use a custom location as your workspace, you can set the *GOPATH* environment variable.

## Learning Go

### A Tour of Go

#### Packages

Every Go program is made up of packages.

Programs start running in package main.

By convention, the package name is the same as the last element of the import path.

#### Imports

"factored" import statement: groups the imports into a parenthesized.

    import (
        "fmt"
        "math"
    )

#### Exported names

In Go, a name is exported if it begins with a capital letter.

When importing a package, you can refer only to its exported names.

#### Functions

The type comes after the variable name.

When two or more consecutive named function parameters share a type, you can omit the type from all but the last.

    func add(x, y int) int {
	    return x + y
    }

#### Multiple results

A function can return any number of results.

    func swap(x, y string) (string, string) {
	    return y, x
    }

#### Named return values

Go's return values may be named. If so, they are treated as variables defined at the top of the function.

A return statement without arguments returns the named return values. This is known as a "naked" return.

    func split(sum int) (x, y int) {
        x = sum * 4 / 9
        y = sum - x
        return
    }

#### Variables

The *var* statement declares a list of variables; as in function argument lists, the type is last.

A *var* statement can be at package or function level.

    package main

    import "fmt"

    var c, python, java bool

    func main() {
        var i int
        fmt.Println(i, c, python, java)
    }

#### Variables with initializers

If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

    package main

    import "fmt"

    var i, j int = 1, 2

    func main() {
        var c, python, java = true, false, "no!"
        fmt.Println(i, j, c, python, java)
    }

#### Short variable declarations

Inside a function, the := short assignment statement can be used in place of a var declaration with implicit type.

    k := 3
    c, python, java := true, false, "no!"

Outside a function, every statement begins with a keyword (var, func, and so on) and so the := construct is not available.

#### Basic types

Go's basic types are:

* bool
* string
* int  int8  int16  int32  int64
* uint uint8 uint16 uint32 uint64 uintptr
* byte // alias for uint8
* rune // alias for int32, represents a Unicode code point
* float32 float64
* complex64 complex128

#### Zero values

Variables declared without an explicit initial value are given their zero value.

The zero value is:

* 0 for numeric types,
* false for the boolean type, and
* "" (the empty string) for strings.

#### Type conversions

The expression T(v) converts the value v to the type T.

Unlike in C, in Go assignment between items of different type requires an explicit conversion.

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
