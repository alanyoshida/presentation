---
title: "A breaf introduction to golang"
author: "Alan Yoshida"
format: revealjs
---

# A breaf introduction to Golang

----

### Installing Go

```bash
// Download tar.gz
wget https://go.dev/dl/go1.19.3.linux-amd64.tar.gz

// extract
tar -C /usr/local -xzf go1.19.3.linux-amd64.tar.gz

// Export binaries path
export PATH=$PATH:/usr/local/go/bin

// Check installation
go version
```

---

### Creating a new project

```bash
go mod init golang
```

---

### Packages
Programs start running in package main
```go
package main
import "fmt"
func main(){
	fmt.Println("Hello World")
}
```

---

### Import
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

### Multiple files in same package
```go
//file hello.go
package main
import "fmt"
func HelloWorld(){
	fmt.Println("Hello World")
}
```

```go
//file main.go
package main
func main(){
	newpack.HelloWorld()
}
```

---

### Dependencies

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

### Comments

```go
// This is a commment
/* This is a comment */
```

---

### Constants

```go
const value int32
const world = "World"
```

---

### Variables

```go
var x interface{}  // x is nil and has static type interface{}
var v *T           // v has value nil, static type *T
x = 42             // x has value 42 and dynamic type int
x = v              // x has value (*T)(nil) and dynamic type *T
```

---

### Types

```go

// Boolean
var boolean bool
boolean = true

another_bool := false

var another_one bool = true

// Numeric
uint8       // the set of all unsigned  8-bit integers (0 to 255)
uint16      // the set of all unsigned 16-bit integers (0 to 65535)
uint32      // the set of all unsigned 32-bit integers (0 to 4294967295)
uint64      // the set of all unsigned 64-bit integers (0 to 18446744073709551615)

int8        // the set of all signed  8-bit integers (-128 to 127)
int16       // the set of all signed 16-bit integers (-32768 to 32767)
int32       // the set of all signed 32-bit integers (-2147483648 to 2147483647)
int64       // the set of all signed 64-bit integers (-9223372036854775808 to 9223372036854775807)

float32     // the set of all IEEE-754 32-bit floating-point numbers
float64     // the set of all IEEE-754 64-bit floating-point numbers

complex64   // the set of all complex numbers with float32 real and imaginary parts
complex128  // the set of all complex numbers with float64 real and imaginary parts

byte        // alias for uint8
rune        // alias for int32
```

---

### Type Inference
```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

---

### Strings
A _string type_ represents the set of string values. A string value is a (possibly empty) sequence of bytes. The number of bytes is called the length of the string and is never negative. Strings are immutable: once created, it is impossible to change the contents of a string. The predeclared string type is `string`; it is a [defined type](https://go.dev/ref/spec#Type_definitions).

```go
// STRINGS
var name string
name = "Alan"

another_name := "Sagan"
```

---

### Array
An array is a numbered sequence of elements of a single type, called the element type. The number of elements is called the length of the array and is never negative.
```go
var integer_array [10]int
var byte_array [32]byte
```

---

### Slice
A slice is a descriptor for a contiguous segment of an _underlying array_ and provides access to a numbered sequence of elements from that array. A slice type denotes the set of all slices of arrays of its element type. The number of elements is called the length of the slice and is never negative. The value of an uninitialized slice is `nil`.

```go
// Declaring
var slice []int
// initializing slice
slice = make([]int, 50, 100)
```

---

### Struct
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

### Pointer
A pointer type denotes the set of all pointers to [variables](https://go.dev/ref/spec#Variables) of a given type, called the _base type_ of the pointer. The value of an uninitialized pointer is `nil`.
```go
var pointer *[4]int
```

---

### Function
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

---

### Interface
An interface type defines a _type set_. A variable of interface type can store a value of any type that is in the type set of the interface. Such a type is said to [implement the interface](https://go.dev/ref/spec#Implementing_an_interface). The value of an uninitialized variable of interface type is `nil`.

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

### Interface Example
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

### Map
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

### For
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

### IF
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

### Switch Case
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

### Defer
```go
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}
```

---

### Pointers
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

---

### Pointers and Methods
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

### Go tests

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
then execute:
```bash
$ go test
PASS
ok      gotest  0.001s
```

---

# That's it folks

Go see the [A tour of Go](https://go.dev/tour/list)
