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

#### Range

The *range* form of the for loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

    var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
    for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}

You can skip the index or value by assigning to _.

If you only want the index, drop the ", value" entirely.

    pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}

#### Maps

The zero value of a map is *nil*. A *nil* map has no keys, nor can keys be added.

The make function returns a map of the given type, initialized and ready for use.

    var m map[string]Vertex
    m = make(map[string]Vertex)

Map literals

    var m = map[string]Vertex{
        "Bell Labs": Vertex{
            40.68433, -74.39967,
        },
        "Google": Vertex{
            37.42202, -122.08408,
        },
    }

    // If the top-level type is just a type name, you can omit it from the elements of the literal.
    var m = map[string]Vertex{
        "Bell Labs": {40.68433, -74.39967},
        "Google":    {37.42202, -122.08408},
    }

Insert or update an element in map m:

    m[key] = elem

Retrieve an element:

    elem = m[key]

Delete an element:

    delete(m, key)

Test that a key is present with a two-value assignment:

    elem, ok := m[key]

* If key is in m, ok is true. If not, ok is false.
* If key is not in the map, then elem is the zero value for the map's element type.

#### Function values

Functions are values too. They can be passed around just like other values.

Function values may be used as function arguments and return values.

#### Function closures

A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.

For example, the adder function returns a closure. Each closure is bound to its own sum variable.

    func adder() func(int) int {
        sum := 0
        return func(x int) int {
            sum += x
            return sum
        }
    }

    func main() {
        pos, neg := adder(), adder()
        for i := 0; i < 10; i++ {
            fmt.Println(
                pos(i),
                neg(-2*i),
            )
        }
    }

#### Methods

Go does not have classes. However, you can define methods on types.

A method is a function with a special receiver argument.

The receiver appears in its own argument list between the func keyword and the method name.

    type Vertex struct {
        X, Y float64
    }

    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }

    func main() {
        v := Vertex{3, 4}
        fmt.Println(v.Abs())
    }

You can declare a method on non-struct types, too.

You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as *int*).

    type MyFloat float64

    func (f MyFloat) Abs() float64 {
        if f < 0 {
            return float64(-f)
        }
        return float64(f)
    }

#### Pointer receivers

You can declare methods with pointer receivers.

This means the receiver type has the literal syntax *T for some type T. (Also, T cannot itself be a pointer such as *int.)

    type Vertex struct {
        X, Y float64
    }

    func (v *Vertex) Scale(f float64) {
        v.X = v.X * f
        v.Y = v.Y * f
    }

Methods with pointer receivers can modify the value to which the receiver points. Since methods often need to modify their receiver, pointer receivers are more common than value receivers.

With a value receiver, the method operates on a copy of the original value.

#### Methods and pointer indirection

Methods with pointer receivers take either a value or a pointer as the receiver when they are called:

    func (v *Vertex) Scale(f float64) {
        v.X = v.X * f
        v.Y = v.Y * f
    }

    var v Vertex
    v.Scale(5)  // OK
    p := &v
    p.Scale(10) // OK

Methods with value receivers take either a value or a pointer as the receiver when they are called:

    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }

    var v Vertex
    fmt.Println(v.Abs()) // OK
    p := &v
    fmt.Println(p.Abs()) // OK

#### Choosing a value or pointer receiver

There are two reasons to use a pointer receiver.

* The first is so that the method can modify the value that its receiver points to.
* The second is to avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.

In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both.

#### Interfaces

An interface type is defined as a set of method signatures.

A value of interface type can hold any value that implements those methods.

    type Abser interface {
        Abs() float64
    }

A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.

Interface values can be thought of as a tuple of a value and a concrete type:

    (value, type)

An interface value holds a value of a specific underlying concrete type.

Calling a method on an interface value executes the method of the same name on its underlying type.

If the concrete value inside the interface itself is *nil*, the method will be called with a nil receiver.

In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a *nil* receiver (as with the method M in this example.)

    func (t *T) M() {
        if t == nil {
            fmt.Println("<nil>")
            return
        }
        fmt.Println(t.S)
    }

Note that an interface value that holds a *nil* concrete value is itself non-nil.

A nil interface value holds neither value nor concrete type. Calling a method on a nil interface is a run-time error.

The interface type that specifies zero methods is known as the *empty interface*:

    interface{}

An *empty interface* may hold values of any type. Empty interfaces are used by code that handles values of unknown type.

#### Type assertions

A type assertion provides access to an interface value's underlying concrete value.

    // This statement asserts that the interface value i holds the concrete type T
    // and assigns the underlying T value to the variable t.
    // If i does not hold a T, the statement will trigger a panic.
    t := i.(T)

To test whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

    t, ok := i.(T)

If i holds a T, then t will be the underlying value and ok will be true.

If not, ok will be false and t will be the zero value of type T, and no panic occurs.

#### Type switches

A type switch is a construct that permits several type assertions in series.

    switch v := i.(type) {
    case T:
        // here v has type T
    case S:
        // here v has type S
    default:
        // no match; here v has the same type as i
    }

#### Stringers

One of the most ubiquitous interfaces is *Stringer* defined by the *fmt* package.

    type Stringer interface {
        String() string
    }

#### Errors

Go programs express error state with error values. The error type is a built-in interface similar to fmt.Stringer:

    type error interface {
        Error() string
    }

A *nil* error denotes success; a non-nil error denotes failure.

#### Readers

The *io* package specifies the *io.Reader* interface, which represents the read end of a stream of data.

    func (T) Read(b []byte) (n int, err error)

*Read* populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an *io.EOF* error when the stream ends.

#### Goroutines

A *goroutine* is a lightweight thread managed by the Go runtime.

    go f(x, y, z)

The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine.

#### Channels

Channels are a typed conduit through which you can send and receive values with the channel operator, <-.

    ch := make(chan int)
    ch <- v     // Send v to channel ch.
    v := <-ch   // Receive from ch, and assign value to v.

The data flows in the direction of the arrow.

By default, sends and receives block until the other side is ready.

#### Buffered Channels

Channels can be buffered. Provide the buffer length as the second argument to make to initialize a buffered channel:

    ch := make(chan int, 100)

Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.

#### Range and Close

A sender can *close* a channel to indicate that no more values will be sent. Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression:

    close(ch)
    v, ok := <-ch

ok is *false* if there are no more values to receive and the channel is closed.

Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.

The loop *for i := range ch* receives values from the channel repeatedly until it is closed.

    c := make(chan int, 10)
	for i := range c {
		fmt.Println(i)
	}

Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver **must** be told there are no more values coming.

#### Select

The *select* statement lets a goroutine wait on multiple communication operations.

A *select* blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

The *default* case in a *select* is run if no other case is ready.

Use a *default* case to try a send or receive without blocking:

    c, quit chan int

    select {
    case c <- x:
        x, y = y, x+y
    case <-quit:
        fmt.Println("quit")
        return
    default:
        // receiving from c would block
    }

#### sync.Mutex

    mux sync.Mutex
    mux.Lock()
	defer mux.Unlock()

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

### Effective Go

#### Introduction

#### Formatting

*go fmt* or *gofmt* operates at the package level rather than source file level.

#### Commentary

 Comments that appear before top-level declarations, with no intervening newlines, are extracted along with the declaration to serve as explanatory text for the item.

 Every package should have a package comment, a block comment preceding the package clause. For multi-file packages, the package comment only needs to be present in one file, and any one will do. The package comment should introduce the package and provide information relevant to the package as a whole.

 One adjustment *godoc* does do is to display indented text in a fixed-width font, suitable for program snippets.

 Inside a package, any comment immediately preceding a top-level declaration serves as a doc comment for that declaration. Every exported (capitalized) name in a program should have a doc comment.

 The first sentence should be a one-sentence summary that starts with the name being declared.

 If every doc comment begins with the name of the item it describes, the output of godoc can usefully be run through grep.

 #### Names

 The visibility of a name outside a package is determined by whether its first character is upper case.

 By convention, packages are given lower case, single-word names; there should be no need for underscores or mixedCaps.

 The package name is only the default name for imports; it need not be unique across all source code, and in the rare case of a collision the importing package can choose a different name to use locally.

 Another convention is that the package name is the base name of its source directory.

 Don't use the *import .* notation, which can simplify tests that must run outside the package they are testing, but should otherwise be avoided.

Go doesn't provide automatic support for getters and setters. It's neither idiomatic nor necessary to put Get into the getter's name.

    owner := obj.Owner()
    if owner != user {
        obj.SetOwner(user)
    }

By convention, one-method interfaces are named by the method name plus an -er suffix or similar modification to construct an agent noun: Reader, Writer, Formatter, CloseNotifier etc.

If your type implements a method with the same meaning as a method on a well-known type, give it the same name and signature.

#### Semicolons

#### Control structures

In a := declaration a variable v may appear even if it has already been declared, provided:

* this declaration is in the same scope as the existing declaration of v (if v is already declared in an outer scope, the declaration will create a new variable §),
* the corresponding value in the initialization is assignable to v, and
* there is at least one other variable in the declaration that is being declared anew.

If you're looping over an array, slice, string, or map, or reading from a channel, a *range* clause can manage the loop.

If you only need the first item in the range (the key or index), drop the second:

    for key := range m {
        if key.expired() {
            delete(m, key)
        }
    }

If you only need the second item in the range (the value), use the blank identifier, an underscore, to discard the first.

Go has no comma operator and ++ and -- are statements not expressions. Thus if you want to run multiple variables in a for you should use parallel assignment (although that precludes ++ and --).

    for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
        a[i], a[j] = a[j], a[i]
    }

There is no automatic fall through, but cases can be presented in comma-separated lists.

    func shouldEscape(c byte) bool {
        switch c {
        case ' ', '?', '&', '=', '#', '+', '%':
            return true
        }
        return false
    }

#### Functions

#### Data

*new(T)* allocates zeroed storage for a new item of type T and returns its address, a value of type *T.

*make* applies only to maps, slices and channels and does not return a pointer.

Arrays are values. Assigning one array to another copies all the elements.

In particular, if you pass an array to a function, it will receive a copy of the array, not a pointer to it.

The size of an array is part of its type. The types [10]int and [20]int are distinct.

When printing a struct, the modified format *%+v* annotates the fields of the structure with their names, and for any value the alternate format *%#v* prints the value in full Go syntax.

If you want to control the default format for a custom type, all that's required is to define a method with the signature *String()* string on the type.

#### Initialization

A common use of *init* functions is to verify or repair correctness of the program state before real execution begins.

#### Methods

#### Interfaces and other types

    str, ok := value.(string)
    if ok {
        fmt.Printf("string value is: %q\n", str)
    } else {
        fmt.Printf("value is not a string\n")
    }

If a type exists only to implement an interface and will never have exported methods beyond that interface, there is no need to export the type itself.
