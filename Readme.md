# Go Basics: Pointers and Structs

This document explains two core Go concepts with examples:

1. Pointers
2. Structs

## 1. Pointers

By default, Go uses pass-by-value. That means when you pass a variable to a function, Go creates a copy.

A pointer is a variable that stores the memory address of another variable. Pointers help you:

- avoid unnecessary copying
- update original data from functions
- work efficiently with large values

### Pointer Example

```go
package main

import "fmt"

func main() {
    x := 10
    p := &x // p points to x

    fmt.Printf("Value of x: %d\n", x)
    fmt.Printf("Address of x: %p\n", &x)
    fmt.Printf("Value of p (address): %p\n", p)
    fmt.Printf("Value at p: %d\n", *p)

    *p = 20 // update x through pointer

    fmt.Printf("New value of x: %d\n", x)
}
```

### Pointer Operators

- `&` (address-of): gets the memory address of a variable
- `*` (dereference): gets or updates the value at a memory address

### Pass by Value vs Pointer

```go
type User struct {
    Username string
}

func UpdateNameValue(u User) {
    u.Username = "NewName" // only changes local copy
}

func UpdateNamePointer(u *User) {
    u.Username = "NewName" // changes original value
}
```

## 2. Structs

Structs are typed collections of fields. They help model real-world entities by grouping related data.

Note about visibility:

- capitalized names (for example, `User`) are exported (public)
- lowercase names (for example, `user`) are unexported (package-private)

### Basic Struct Example

```go
package main

import "fmt"

type User struct {
    ID       int
    Username string
    Email    string
    IsActive bool
}

func main() {
    u := User{ID: 1, Username: "gopher123", Email: "go@example.com", IsActive: true}
    fmt.Println(u.Username)
}
```

### Struct Embedding (Composition)

Go does not use class inheritance. Instead, it uses embedding to promote fields and methods.

```go
package main

import "fmt"

type User struct {
    Username string
}

type Admin struct {
    User  // embedded struct
    Level int
}

func main() {
    a := Admin{
        User:  User{Username: "admin01"},
        Level: 10,
    }

    fmt.Println(a.Username) // promoted field from embedded User
}
```

### Methods on Structs

You can attach behavior to structs using methods.

```go
package main

import "fmt"

type User struct {
    Username string
    IsActive bool
}

func (u User) IsActiveUser() bool {
    return u.IsActive
}

func main() {
    u := User{Username: "gopher123", IsActive: true}
    fmt.Println(u.IsActiveUser())
}
```