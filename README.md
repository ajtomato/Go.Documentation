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

#### Type inference

When declaring a variable without specifying an explicit type (either by using the := syntax or var = expression syntax), the variable's type is inferred from the value on the right hand side.

When the right hand side of the declaration is typed, the new variable is of that same type.

But when the right hand side contains an untyped numeric constant, the new variable may be an *int*, *float64*, or *complex128* depending on the precision of the constant.

#### Constants

Constants are declared like variables, but with the const keyword.

    const Pi = 3.14

Constants cannot be declared using the := syntax.

#### Numeric Constants

Numeric constants are high-precision values.

An untyped constant takes the type needed by its context.

#### For

Go has only one looping construct, the *for* loop.

    for i := 0; i < 10; i++ {
		sum += i
	}

    for ; sum < 1000; {
		sum += sum
	}

    for sum < 1000 {
		sum += sum
	}

    // Forever
    for {
	}

The braces { } are always required.

#### If

Go's *if* statements are like its *for* loops; the expression need not be surrounded by parentheses ( ) but the braces { } are required.

    if x < 0 {
		return sqrt(-x) + "i"
	}

    if v := math.Pow(x, n); v < lim {
		return v
	}

    if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}

#### Switch

Go only runs the selected case, not all the cases that follow.

Go's switch cases need not be constants, and the values involved need not be integers.

    switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}

Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

Switch without a condition is the same as switch true. This construct can be a clean way to write long if-then-else chains.

    t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}

#### Defer

A *defer* statement defers the execution of a function until the surrounding function returns.

    defer fmt.Println("world")

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

#### Pointers

A pointer holds the memory address of a value.

The type *T is a pointer to a T value. Its zero value is nil.

    var p *int
    i := 42
    p = &i
    fmt.Println(*p) // read i through the pointer p
    *p = 21         // set i through the pointer p

#### Structs

A *struct* is a collection of fields.

    type Vertex struct {
	    X int
	    Y int
    }

    v := Vertex{1, 2}
	v.X = 4
    p := &v
	p.X = 1e9

To access the field X of a struct when we have the struct pointer p we could write (*p).X. However, that notation is cumbersome, so the language permits us instead to write just p.X, without the explicit dereference.

A *struct literal* denotes a newly allocated struct value by listing the values of its fields. You can list just a subset of fields by using the Name: syntax. (And the order of named fields is irrelevant.) The special prefix & returns a pointer to the struct value.

    v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex

#### Arrays

The type *[n]T* is an array of n values of type T.

An array's length is part of its type, so arrays cannot be resized.

    var a [10]int

#### Slices

The type *[]T* is a slice with elements of type T. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

A slice is formed by specifying two indices, a low and high bound, separated by a colon:

    a[low : high]

This selects a half-open range which includes the first element, but excludes the last one.

A slice does not store any data, it just describes a section of an underlying array. Changing the elements of a slice modifies the corresponding elements of its underlying array.

A *slice literal* is like an array literal without the length.

    q := []int{2, 3, 5, 7, 11, 13}

When slicing, you may omit the high or low bounds to use their defaults instead. The default is zero for the low bound and the length of the slice for the high bound.

    var a [10]int
    
    // these slice expressions are equivalent:
    a[0:10]
    a[:10]
    a[0:]
    a[:]

A slice has both a *length* and a *capacity*. The *length* of a slice is the number of elements it contains. The *capacity* of a slice is the number of elements in the underlying array, counting from the first element in the slice.

    s := []int{2, 3, 5, 7, 11, 13}
    fmt.Printf("len=%d cap=%d %v\n", len(s[1:3]), cap(s[2:3]), s)

The zero value of a slice is *nil*. A *nil slice* has a length and capacity of 0 and has no underlying array.

Slices can be created with the built-in *make* function; this is how you create dynamically-sized arrays. The *make* function allocates a **zeroed** array and returns a slice that refers to that array. To specify a capacity, pass a third argument to make.

    a := make([]int, 5)     // len(a)=5
    b := make([]int, 0, 5)  // len(b)=0, cap(b)=5

It is common to append new elements to a slice, and so Go provides a built-in *append* function.

    func append(s []T, vs ...T) []T

    var s []int

	// append works on nil slices.
	s = append(s, 0)

	// The slice grows as needed.
	s = append(s, 1)

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)

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
