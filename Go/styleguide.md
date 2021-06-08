# Golang Style Guide

This golang guide is indended for coaldev developers.
Read the guide and follow these guidlines in your code.
If you have any better suggestion, reach out to us!

## index

* [Gofmt](#gofmt)
* [Comment Sentences](#comment-sentences)
* [Naming Conventions](#naming-conventions)
* [Imports](#imports)
* [Unit Tests](#unit-tests)
* [Error Handling](#error-handling)
* [Panic](#panic)
* [Structure](#structure)
* [Configuration](#configuration)


## Gofmt

Run [gofmt](https://golang.org/cmd/gofmt/) on your code to automatically fix the majority of formatting issues. The gofmt program reads a Go program and emits the source in a standard style of indentation and vertical alignment, retaining and if necessary reformatting comments.

## Comment Sentences

Go provides C-style /* */ block comments and C++-style // line comments. Every function, interface or type should have Doc comment. The first sentence should be a one-sentence summary that starts with the name being declared. Doc comments work best as complete sentences, which allow a wide variety of automated presentations.

```go
// Request represents a request to run a command.
type Request struct { ...

// Encode writes the JSON encoding of req to w.
func Encode(w io.Writer, req *Request) { ...
```

## Naming Conventions

The visibility of a name outside a package is determined by whether its first character is upper case. Finally, the convention in Go is to use MixedCaps or mixedCaps rather than underscores to write multiword names.

## Imports

Avoid renaming imports except to avoid a name collision; good package names
should not require renaming. In the event of collision, prefer to rename the most
local or project-specific import.


Imports are organized in groups, with blank lines between them.
The standard library packages are always in the first group.

```go
package main

import (
	"fmt"
	"hash/adler32"
	"os"

	"appengine/foo"
	"appengine/user"

	"github.com/foo/bar"
	"rsc.io/goversion/version"
)
```

## Unit Tests

Always write a unit test for your function in the filename_test.go file. Here the 'filename' is the file you are writing your function in.  It is intended to be used in concert with the "go test" command, which automates execution of any function of the form.

```go
func TestXxx(*testing.T)
```

where Xxx does not start with a lowercase letter. The function name serves to identify the test routine.

```go
func TestAbs(t *testing.T) {
    got := Abs(-1)
    if got != 1 {
        t.Errorf("Abs(-1) = %d; want 1", got)
    }
}
```

## Error Handling

Go's multivalue return makes it easy to return a detailed error description alongside the normal return value.

```go
// Hello returns a greeting for the named person.
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message.
    if name == "" {
        return "", errors.New("empty name")
    }

    // If a name was received, return a value that embeds the name
    // in a greeting message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message, nil
}

message, err := Hello("")
if err != nil {
    log.Fatal(err)
}
```

 Do not discard errors using `_` variables. If a function returns an error, check it to make sure the function succeeded. Handle the error, return it, or, in truly exceptional situations, panic.

## Panic

Sometimes the program simply cannot continue. In this case, use panic function to stop the execution of code. If the problem can be masked or worked around, it's always better to let things continue to run rather than taking down the whole program.

 ```go
var user = os.Getenv("USER")

func init() {
    if user == "" {
        panic("no value for $USER")
    }
}
```

## Structure

Golang does not have classes so use struct for data members and use functions for methods. Declare Stucts on the top of file. A struct is a collection of data fields with declared data types. Golang has the ability to declare and create own data types by combining one or more types, including both built-in and user-defined types.

```go
type Profitability struct {
	ROCE float64
	ROE  float64
	NPM  float64
	ROA  float64
}
```

## Configuration

If a program requires configurations. Create json file and place configurations in this file. In go, declare global configurations struct and use it accross the project.