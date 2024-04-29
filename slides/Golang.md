---
title: "A breaf introduction to golang"
author: "Alan Yoshida"
format:
  revealjs:
    transition: slide
    background-transition: fade
    logo: "golang/gopher.svg"
    footer: "golang is awesome"
---


## History {.center}
It was developed in 2007 by Robert Griesemer, Rob Pike and Ken Thompson at Google but launched in 2009 as an open-source programming language.

![](golang/gopher.svg){fig-align="center" height=200}


---


## Why Golang {.center}

Concurrency is very hard in C, golang was born the help in this sense.


---

## Main characteristics {.center}

- Compiled
- Garbage Collection
- Static type checking
- Syntax similar to C
- Built-in concurrency primitives (goroutines, channels)
- Emphasis on greater simplicity and safety

---

## Installing Go {.center}

```bash
// Download tar.gz
wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz

// extract
tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz

// Export binaries path
// Add to .profile, .bashrc or .zshrc or config.fish
export PATH=$PATH:/usr/local/go/bin

// Check installation
go version
```

---

## GOPATH {.center}

Go development using dependencies beyond the standard library is done using Go modules.

When using Go modules, the `GOPATH` variable (which defaults to `$HOME/go` on Unix and `%USERPROFILE%\go` on Windows) is used for the following purposes:

---

## GOBIN {.center}

- The `go install` command installs binaries to `$GOBIN`, which defaults to `$GOPATH/bin`.
- The `go get` command caches downloaded modules in `$GOMODCACHE`, which defaults to `$GOPATH/pkg/mod`.
- The `go get` command caches downloaded checksum database state in `$GOPATH/pkg/sumdb`.


---

## Go Modules {.center}

A module is a collection of Go packages stored in a file tree with a `go.mod` file at its root.

The `go.mod` file defines the module’s module path, which is also the import path used for the root directory of the project, and its dependency requirements, which are the other modules needed for a successful build. Each dependency requirement is written as a module path and a specific semantic version.

---

## Creating a new project {.center}

```bash
go mod init example.com/hello
```

---

## Packages {.center}

Programs start running in package main
```go
package main
import "fmt"
func main(){
  fmt.Println("Hello World")
}
```

---

## Import {.center}
Import other modules
```go
// one by one
import "fmt"
import "math"

// All at once
import (
  "fmt"
  "math"
)

```

---

## Multiple files in same package {.center}

:::: {.columns }

::: {.column width="50%"}
```go
//file hello.go
package main
import "fmt"
func HelloWorld(){
  fmt.Println("Hello World")
}
```
:::
::: {.column width="50%"}
```go
//file main.go
package main
func main(){
  newpack.HelloWorld()
}
```
:::
::::

---

## Multiple packages

:::: {.columns }
::: {.column width="50%"}
cmd/main.go
```go
package main
import (
  mypackages "example.com/m/pkgs/myPackages"
)
func main() {
  mypackages.PublicInPackages()
}
```
:::
::: {.column width="50%"}
pkgs/myPackages/functions.go
```go
package mypackages
import "fmt"
func PublicInPackages() {
  fmt.Println("In Public Function")
  privateInPackages()
}
func privateInPackages() {
  fmt.Println("In private function")
}
```
:::
::::

---

## Dependencies {.center}

You can set your dependencies in go.mod file

go.mod
```go
module github.com/alanyoshida/meuprojeto

go 1.18

require (
  github.com/go-delve/delve v1.5.0
  github.com/gofiber/fiber v1.14.6
  github.com/sirupsen/logrus v1.7.0
  github.com/spf13/cobra v1.1.1
)
```

---

## Golang Commands {.center}

Build the project to a binary:

`go build .`

Run without generating a binary:

`go run main.go`

Execute tests:

`go test` or `go test ./...`

Install binaries from github:

`go install github.com/norwoodj/helm-docs/cmd/helm-docs@latest`

---

## Building for containers {.center}

Depending on the container you must compile with different libs, like alpine that only suports musl. In that case you must compile the golang binary inside the container for better compatibility.

---

## Comments {.center}

```go
// This is a commment
/* This is a comment */
```

---

## Constants {.center}

```go
const value int32
const world = "World"
```

---

## Variables {.center}

```go
var x interface{}  // x is nil and has static type interface{}
var v *T           // v has value nil, static type *T
x = 42             // x has value 42 and dynamic type int
x = v              // x has value (*T)(nil) and dynamic type *T
```

---

## Types {.center}


```go
// Boolean
var boolean bool // Declaration only
boolean = true // Attribution only
another_bool := false // Declare and attribute infering type
var another_one bool = true // Declare type and Atribution

complex64   // complex numbers with float32 real and imaginary parts
complex128  // complex numbers with float64 real and imaginary parts

byte        // alias for uint8
rune        // alias for int32
```


---

## Types {.center}

`unsigned` is only for positive numbers

```go
// Numeric
uint8  // unsigned  8-bit integers (0 to 255)
uint16 // unsigned 16-bit integers (0 to 65535)
uint32 // unsigned 32-bit integers (0 to 4294967295)
uint64 // unsigned 64-bit integers (0 to 18446744073709551615)
int8   // signed  8-bit integers (-128 to 127)
int16  // signed 16-bit integers (-32768 to 32767)
int32  // signed 32-bit integers (-2147483648 to 2147483647)
int64  // signed 64-bit integers (-9223372036854775808 to 9223372036854775807)

float32     // IEEE-754 32-bit floating-point numbers
float64     // IEEE-754 64-bit floating-point numbers
```

---

## Type Inference {.center}

Using `:=` the type is inferred automatically

```go
package main
import (
    "fmt"
    "os"
)
func main() {
    i := 42               // int
    f := 3.142            // float64
    g := 0.867 + 0.5i     // complex128
    another_bool := false // boolean
    number := returnInt() // number is int
    fmt.Fprintf(os.Stdout, "number: %T\n", number)
    // out: number: int
}
func returnInt() int {
    return 3
}
```

---

## Strings {.center}
A _string type_ represents the set of string values.
A string value is a (possibly empty) sequence of bytes.
The number of bytes is called **the length of the string** and is never negative.
Strings are immutable: once created, it is impossible to change the contents of a string. The predeclared string type is `string`; it is a [defined type](https://go.dev/ref/spec#Type_definitions).

```go
// STRINGS
var name string
name = "Alan"

// Infering type automatically
another_name := "Sagan"
```

---

## Array {.center}
An array is a numbered sequence of elements of a single type, called the element type. The number of elements is called the length of the array and is never negative.
```go
var integer_array [10]int
var byte_array [32]byte

var intArr1 [3]int32 // Declaring array of size 3
intArr1[1] = 123 // Set value at index
fmt.Println(intArr[0]) // Accessing array at index

var intArr2 [3]int32 = [3]int32{1,2,3} // Declare an set values

intArr2 := [3]int32{1,2,3} // Infering type and set value

intArr := [...]int32{1,2,3} // Infering size with ...

```

---

## Slice {.center}
A slice is a descriptor for a contiguous segment of an _underlying array_ and provides access to a numbered sequence of elements from that array.

A slice type denotes the set of all slices of arrays of its element type.

The value of an uninitialized slice is `nil`.

```go
// Declaring
var slice []int = make([]int, 50, 100)

var intSlice []int32 = []int32{4, 5, 6}
intSlice = append(intSlice, 7)

intSlice2 := []int32{8, 9, 10}
intSlice = append(intSlice, intSlice2...) // Spread operator ...
fmt.Println(intSlice)

```

---

## Struct {.center}
A struct is a sequence of named elements, called fields, each of which has a name and a type.
```go
// An empty struct.
struct {}

// A struct with 6 fields.
struct {
  x, y int
  u float32
  _ float32  // padding
  A *[]int
  F func()
}
```

---

## Function {.center}
A function type denotes the set of all functions with the same parameter and result types. The value of an uninitialized variable of function type is `nil`.
```go
func()
func(x int) int
func(a, _ int, z float32) bool
func(a, b int, z float32) (bool)
func(prefix string, values ...int)
func(a, b int, z float64, opt ...interface{}) (success bool)
func(int, int, float64) (float64, *[]int)
func(n int) func(p *T)
```
Golang can return multiple values

---

## Ignore returned value {.center}
"`_`" ignores the returned value
```go
func main() {
  returnedInt, _ := multipleReturns()
  fmt.Printf("Returned %d", returnedInt)
}
func multipleReturns() (int, string) {
  return 3, "My String"
}
```

---

## Error Handling {.center}
This is a common pattern of error handling in golang
```go
func main() {
  result, err := withError(200)
  if err != nil {
    fmt.Fprintf(os.Stderr, "%v\n", err.Error())
    os.Exit(1)
  }
  fmt.Fprintf(os.Stdout, "%v", result)
}

func withError(age int) (string, error) {
  if age > 150 {
    return "", errors.New("Error: Could not live that long")
  }
  return fmt.Sprintf("Your age is %d", age), nil
}
```

---

## Interface {.center}
An interface type defines a _type set_. A variable of interface type can store a value of any type that is in the type set of the interface. Such a type is said to [implement the interface](https://go.dev/ref/spec#Implementing_an_interface). The value of an uninitialized variable of interface type is `nil`.

---

## File interface {.center}
```go
// A simple File interface.
type FileManager interface {
  Read([]byte) (int, error)
  Write([]byte) (int, error)
  Close() error
}

// Implement interface
func (p MyFile) Read(p []byte) (n int, err error)
func (p MyFile) Write(p []byte) (n int, err error)
func (p MyFile) Close() error
```

---

## Interface Example {.center}
```go
package main
import "fmt"

type I interface {
  M()
}
type T struct {
  S string
}
// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
  fmt.Println(t.S)
}
func main() {
  var i I = T{"hello"}
  i.M()
}
```

---

## Map {.center}
A map is an unordered group of elements of one type, called the element type, indexed by a set of unique _keys_ of another type, called the key type. The value of an uninitialized map is `nil`.

```go
// Declaring
var mymap1 map[string]int
var mymap2 map[*T]struct{ x, y float64 }
var mymap3 map[string]interface{}

// Empty Map
make(map[string]int)
make(map[string]int, 100)
```

---

## For {.center}
```go
for i := 0; i < 10; i++ {
  sum += i
}
sum := 1
for ; sum < 1000; {
  sum += sum
}
// For is Go's "while"
sum := 1
for sum < 1000 {
  sum += sum
}
// Forever For
for {
}
// range
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
for i, v := range pow {
  fmt.Printf("2**%d = %d\n", i, v)
}
for _, value := range pow {
  fmt.Printf("%d\n", value)
}
```

---

## IF {.center}

```go
func sqrt(x float64) string {
  if x < 0 {
    return sqrt(-x) + "i"
  }
  return fmt.Sprint(math.Sqrt(x))
}
func pow(x, n, lim float64) float64 {
  if v := math.Pow(x, n); v < lim {
    return v
  }
  return lim
}
func pow(x, n, lim float64) float64 {
  if v := math.Pow(x, n); v < lim {
    return v
  } else {
    fmt.Printf("%g >= %g\n", v, lim)
  }
  // can't use v here, though
  return lim
}
```

---

## Switch Case {.center}

```go
func main() {
  fmt.Print("Go runs on ")
  switch os := runtime.GOOS; os {
  case "darwin":
    fmt.Println("OS X.")
  case "linux":
    fmt.Println("Linux.")
  default:
    // freebsd, openbsd,
    // plan9, windows...
    fmt.Printf("%s.\n", os)
  }
}
```

---

## Defer {.center}

A defer statement defers the execution of a function until the surrounding function returns.

```go
func main() {
  defer fmt.Println("world")
  fmt.Println("hello")
}
```

---

## Declaring Pointers {.center}
A pointer type denotes the set of all pointers to [variables](https://go.dev/ref/spec#Variables) of a given type, called the _base type_ of the pointer. The value of an uninitialized pointer is `nil`.
```go
var pointer *[4]int
```

---

## Using Pointers {.center}
:::: {.columns}

::: {.column width="50%" style="font-size: .8em"}
A pointer holds the memory address of a value.

- The `&` get the memory address.
- The `*` get the value stored in a memory address.

This is known as "dereferencing" or "indirecting".

:::
::: {.column width="50%"}
```go
i, j := 42, 2701

p := &i         // point to i
fmt.Println(*p) // read i through the pointer
*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i

p = &j         // point to j
*p = *p / 37   // divide j through the pointer
fmt.Println(j) // see the new value of j
```
:::
::::

---

## Pointers and Methods {.center}

```go
package main
import "fmt"

type Vertex struct {
  X, Y float64
}
func (v *Vertex) Scale(f float64) {
  v.X = v.X * f
  v.Y = v.Y * f
}
func ScaleFunc(v *Vertex, f float64) {
  v.X = v.X * f
  v.Y = v.Y * f
}
func main() {
  v := Vertex{3, 4}
  v.Scale(2)
  ScaleFunc(&v, 10)
  p := &Vertex{4, 3}
  p.Scale(3)
  ScaleFunc(p, 8)
  fmt.Println(v, p)
}
```

---

## Go routines {.center}

A goroutine is a lightweight thread managed by the Go runtime.

```go
// Call a function f() in a thread
go f(x, y, z)
```
The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine.


---

## Channels {.center}

Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```
(The data flows in the direction of the arrow.)

Like maps and slices, channels must be created before use:

```go
ch := make(chan int)
```
By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

---

## Go routine Example {.center}

```go
func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}
```

---

## Buffered Channels {.center}

Channels can be buffered.
```go
func main() {
	ch := make(chan int, 2)
	ch <- 1
	ch <- 2
	fmt.Println(<-ch)
	fmt.Println(<-ch)
}
```

---

## Select {.center}
The select statement lets a goroutine wait on multiple communication operations.

```go
func main(){
  d := make(chan int)
  quit := make(chan bool)
  go func() {
    time.Sleep(1 * time.Second)
    d <- 1
  }()
  go func() {
    time.Sleep(2 * time.Second)
    d <- 2
  }()
  go func() {
    time.Sleep(3 * time.Second)
    quit <- true
  }()

  for {
    select {
    case msg := <-d:
      fmt.Println(msg)
    case <-quit:
      fmt.Println("Quit")
      return
    }
  }
}
```

---

## Hello world {.center}

main.go
```go
package main
import "fmt"
func Hello() string {
  return "Hello, world"
}
func main() {
  fmt.Println(Hello())
}
```

---

## Test Hello World {.center}

main_test.go
```go
package main
import "testing"
func TestHello(t *testing.T) {
  got := Hello()
  want := "Hello, world"

  if got != want {
    t.Errorf("got %q want %q", got, want)
  }
}
```

---

## Run Tests {.center}
then execute:
```bash
$ go test -race
PASS
ok      gotest  0.001s
```

---

## Test Coverage {.center}

Generate golang test coverage html using the following commands:

```bash
# Generate coverage file
$ go test -race -coverprofile=coverage.out ./...
ok      example.com/m/slides    0.001s  coverage: 50.0% of statements

# Show coverage in command line
$ go tool cover -func=coverage.out
example.com/m/slides/main.go:5: Hello           100.0%
example.com/m/slides/main.go:8: main            0.0%
total:                          (statements)    50.0%

# Show coverage in HTML
$ go tool cover -html=coverage.out
```

# That's all folks

Go see:

- [A tour of Go](https://go.dev/tour/list)
- [Learn go with tests](https://quii.gitbook.io/learn-go-with-tests)
- [Go by Example](https://gobyexample.com/)
