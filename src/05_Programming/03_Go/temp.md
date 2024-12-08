# Go Programming Language

In-depth internals, my personal notes, example codes and projects. Includes - Thousands of codes, OOP, Concurrency, Parallelism, Goroutines, Mutexes & Wait Groups, Testing in Go, Go tool chain, Backend web development, Some projects including Log file parser using bufio.Scanner, Spam Masker, Retro led clock, Console animations, Dictionary programs, Social Network built using Go and GopherJS, Database Connectivity and working (MySQL, MongoDB, Redis), GopherJS and lot more..

## Author
Aditya Hajare ([Linkedin](https://in.linkedin.com/in/aditya-hajare)).

## Current Status
WIP (Work In Progress)!

## License
Open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

----------------------------------------

## Important Notes
- [Go Configurations](#go-configurations)
    ```diff
    + Environment Configurations
    + VS Code Configurations
    ```
- [Basics](#basics)
    ```diff
    + Packages
        - Executable Packages
        - Library Packages
    + Function init()
    + Scopes
    + Renaming Imports
    + Exporting
    + Data Types
        - Basic Data Types
    + Variables
        - Zero Values
        - Unused variables
        - Multiple Declarations
        - Type Inference
        - Short Declaration
        - Multiple Short Declarations
        - Redeclarations With Short Declarations
    + Blank Identifier
    + fmt.Printf and fmt.Sprintf Formatting
    + Slice Vs. Array - Performance
    + Composite Types In Go
    ```
- [Naming Conventions In Go](07-Naming-Conventions/README.md#naming-rules-in-go-language)  :arrow_upper_right:
- [Type System In Go](#type-system-in-go)
    ```diff
    + Important Links
    + Predeclared Types
    + Defined Types
    + Aliased Types
    ```
- [Constants](#constants)
    ```diff
    + Important Links
    + Constant Types
    + Multiple Constants Declaration
    + Typeless Or Untyped Constants
    + Default Types
    + IOTA
    + Common Abbreviations Used In Go
    ```
- [Error Handling](#error-handling)
    ```diff
    + nil
    ```
- [Strings Runes And Bytes](#strings-runes-and-bytes)
    ```diff
    + Important Links
    + Strings Runes And Bytes 101
    ```
- [Maps In Go](#-maps-in-go)
    ```diff
    + Maps 101
    ```
- [Structs In Go](#structs-in-go)
    ```diff
    + Inheritance vs. Composition
    + Structs 101
    ```
- [OOP In Go With Methods And Interfaces](#oop-in-go-with-methods-and-interfaces)
    ```diff
    + Methods
    + Pointer Receivers
    + Attaching Methods To Any Types
    + Interfaces
    + Type Assertion
    + Empty Interface
    + Type Switch
    ```
- [Concurrency And Parallelism](#concurrency-and-parallelism)
    ```diff
    + Concurrency
    + Parallelism
    + Concurrency vs. parallelism
    ```
- [Goroutines](#goroutines)
    ```diff
    + Advantages of Goroutines over Threads
    ```
- [Channels](#channels)
    ```diff
    + Buffered Channels
    ```
- [Mutexes And Wait Groups From GoSync Package](#mutexes-and-wait-groups-from-gosync-package)
    ```diff
    + Mutexes
    + Wait Groups
    ```
- [Go Vet](#go-vet)
- [Go Documentation Server On Local Machine](#go-documentation-server-on-local-machine)

----------------------------------------

## Go Configurations
```diff
+ Environment Configurations
```
- Open up `.profile` or `.zshrc` or `.bashrc` depending on our OS and add/edit following:
    ```bash
    #!/bin/bash

    # Specifies where the Go destribution is installed on the system.
    export GOROOT=/usr/local/go

    # Specifies top-level directory containing source code for all our Go projects.
    # Inside this directory, we need to create 3 more directories viz. "src", "pkg" and "bin".
    export GOPATH=~/adiwork/go  # This directory is also known as Go Workspace.
    # "src" directory inside Workspace represents where all the Go source code will be stored.
    # "pkg" directory inside Workspace represents where the compiled Go packages will be stored.
    # "bin" directory inside Workspace represents where the produced Go compiled binaries will be stored.

    # Specifies where Go should install compiled binaries.
    export GOBIN=${GOPATH}/bin

    # Attaching GOROOT and GOBIN to shell environment's path variable.
    export PATH=${PATH}:/usr/local/bin:${GOROOT}/bin:${GOBIN}
    ```
- Execute following command to get `stringer`:
    ```sh
    go get -u golang.org/x/tools/cmd/stringer
    ```

----------------------------------------

```diff
+ VS Code Configurations
```
- My VS Code configs for Go:
    ```json
    {
        "go.lintTool": "golangci-lint",
        "go.formatTool": "goimports",
        "go.useLanguageServer": true,
        "go.lintOnSave": "package",
        "go.vetOnSave": "package",
        "go.vetFlags": [
            "-all",
            "-shadow"
        ]
    }
    ```

----------------------------------------

## Basics
- Go is a `strongly typed` language. Because of that, it helps Go compiler to identify many types of errors at `compile time` even before our program is run.

```diff
+ Packages
```
- All package files, should be in the same (single) directory. i.e. all package source code files should be located in a one single directory.
- All files in a specific folder should belong to a one single package. It's a convention, not a rule.
> There are 2 kinds of packages in Go: `Executable Packages` and `Library Packages`.
- To make a package executable, name that package `main`. It's a special package.
- `package` clause can be used only once per file and it should be the first line in `.go` source file.
- Package contains multiple `Go` files belonging to same folder.
- Any package that is intended to run on a `command-line`, must declare `package main`.
- To alias a package upon importing:
    ```go
    package main

    import fm "fmt"  // Package "fmt" has been aliased as "fm"

    func main() {
        //
    }
    ```

----------------------------------------

```diff
- Executable Packages
```
- It's name should always be `package main`.
- `Executable Package` should also contain `main()` function and that too only once.
- These are created only for `running` it as a Go program.
- These cannot be imported into a Go program.
- Package name should be `main`.

----------------------------------------

```diff
- Library Packages
```
- Almost all `Go Standard Library Packages` are of type `Library Packages`.
- They are reusable packages.
- They are not executable packages. So we can't run them.
- We can only `import` them.
- These are created only for `reusability` purposes.
- Package name can have any name.
- Doesn't need to have function named `main()`. To avoid confusion, it's better not to have function named `main()` in a reusable package.

----------------------------------------

```diff
+ Function init()
```
- The `init()` function is used to `initialize` the `state of a package`.
- Go automatically calls `init()` function `before` calling `command-line` package's` main()` function.

----------------------------------------

```diff
+ Scopes
```
- Same name cannot be declared again inside a same scope.
- There are following types of scopes in Go:
    1. `package`: Each Go package has it's own `scope`. For e.g. declared `funcs` are only `visible` to the files belonging to same `package`.
    2. `file`: Imported packages are only visible to the importing file. Each file has to import external packages on it's own.
    3. `func`.
    4. `block`.

----------------------------------------

```diff
+ Renaming Imports
```
- We can rename an imported package name with following syntax:
    ```go
    package main

    import "fmt"
    import adi "fmt"    // Imported "fmt" package and renamed it to "adi"

    func main() {
        adi.Println("नमस्ते आदित्य")    // This will print "नमस्ते आदित्य"
    }
    ```
- We can import packages with the same name into same file by giving one of them imports a new name.

----------------------------------------

```diff
+ Exporting
```
- To export a name in Go, just make it's **first letter** an **uppercase letter**.
- For e.g.
    ```go
    package aditest

    func Adi() { // 'Adi()' will be exported and will be available throughout 'aditest' package
        // Code..
    }

    func adiNew() { // 'adiNew()' will not be exported since it's name doesn't start with uppercase letter.
        // Code
    }
    ```

----------------------------------------

```diff
+ Data Types
```
- `literal` means the `value` itself. Unline `variable`, a `literal` doesn't have a name.
- There are following data types in Go:
    * **Basic type**: Numbers, strings, and booleans come under this category.
    * **Aggregate type**: Array and structs come under this category.
    * **Reference type**: Pointers, slices, maps, functions, and channels come under this * category.
    * **Interface type**

----------------------------------------

```diff
- Basic Data Types
```
- Following are the basic data types in Go:
    * **Numeric**:
        ```go
        // Integer Types
        uint8   // Unsigned 8-bit integers (0 to 255)
        uint16  // Unsigned 16-bit integers (0 to 65535)
        uint32  // Unsigned 32-bit integers (0 to 4294967295)
        uint64  // Unsigned 64-bit integers (0 to 18446744073709551615)
        int8    // Signed 8-bit integers (-128 to 127)
        int16   // Signed 16-bit integers (-32768 to 32767)
        int32   // Signed 32-bit integers (-2147483648 to 2147483647)
        int64   // Signed 64-bit integers (-9223372036854775808 to 9223372036854775807)

        // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

        // Floating Types
        float32     // IEEE-754 32-bit floating-point numbers
        float64     // IEEE-754 64-bit floating-point numbers
        complex64   // Complex numbers with float32 real and imaginary parts
        complex128  // Complex numbers with float64 real and imaginary parts

        // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

        // Other Numeric Types
        byte     // same as uint8
        rune     // same as int32
        uint     // 32 or 64 bits
        int      // same size as uint
        uintptr  // an unsigned integer to store the uninterpreted bits of a pointer value
        ```
    * **Boolean**:
        ```go
        bool    // Represents 'true' or 'false'
        ```
    * **String**:
        - In Go language, strings are different from other languages like Java, C++, Python, etc.
        - Strings can't be `null` in Go.
        - It is a sequence of variable-width characters where each and every character is represented by one or more bytes using UTF-8 Encoding.
        - In Go, a `string` is in effect is a read-only slice of bytes (immutable).
        - Or in other words, strings are the immutable chain of arbitrary bytes (including bytes with zero value) and the bytes of the strings can be represented in the Unicode text using UTF-8 encoding.
        - String literals can be created in 2 ways:
            * **Using double quotes**
            * **Using backticks**

----------------------------------------

```diff
+ Variables
```
- [Variables in Go Lang](https://blog.learngoprogramming.com/learn-go-lang-variables-visual-tutorial-and-ebook-9a061d29babe)
- In Go, we have to **declare** a **variable before** we can **use it**. This is required and necessary for the `compile time safety`.
- Variables are not created at `compile time`. They are created at `run time`.
- The unnamed variables are `pointers` (like in C).
- Once we declare a type for a variable, it cannot be changed later. It is static.

----------------------------------------

```diff
- Zero Values
```
- When a variable is declared and it isn't assigned any value at the time of declaration, Go will assign a `zero value` to it based on it's variable type.
- Type of a variable decides what `zero value` it will take initially when declared (and if it isn't assigned any value at the time of declaration).
    ```go
    // Zero Values assigned to variables by Go when they are declared and not assigned any values at the time of declaration.
    var adiBool bool          // false
    var adiInt int            // 0
    var adiFloat float64      // 0
    var adiStr string         // ""
    var adiPointer *string    // nil | 'nil' means it doesn't point to any memory location
    ```

----------------------------------------

```diff
- Unused variables
```
- **Unused variables in `blocked scope` are not allowed in Go since they cause `maintenance nightmares`.** If we declare a variable in `blocked scope` then we must use it or else completely remove it from the block. We cannot have unused variables declared in `blocked scope` dangling in our source codes. Go throws unused variable errors at `compile time` only.
- **We should avoid using `package level` variables.** Go doesn't throw `unused variable errors` at `compile time` for variables declared at `package level`.

----------------------------------------

```diff
- Multiple Declarations
```
- Sometimes it is also called as parallel variable declarations.
- Declaring multiple variables with `different types` in a single statement:
    ```go
    package main

    func main() {
        var (
            adiBool bool
            adiInt int
            adiFloat float64
            adiStr string
            adiPointer *string
        )
    }
    ```
- Declaring multiple variables with `same type` in a single statement:
    ```go
    package main

    func main() {
        var foo, bar, baz int
    }
    ```

----------------------------------------

```diff
- Type Inference
```
- `Type Inference` means Go can figure out the type of a variable automatically from it's assigned value.
- If we are assigning value to a variable at the time of it's declaration, we can ommit it's `type` specification.
- For e.g.
    ```go
    package main

    main() {
        var someFlag = true // We are not specifying type of 'someFlag' as bool here.
    }
    ```

----------------------------------------

```diff
- Short Declaration
```
- With `Type Inference`, Go can figure out variable type based off it's assigned value.
- In `Short Declaration`, we can declare variable by completely ommitting `var` keyword along with it's variable type.
- It `declares` and `initializes` the variable.
- **We cannot use `Short Declaration` syntax to declare variables in `Package Scope`.**
- At `Package Scope`, all declarations should start with a `keyword`. Since `Short Declaration` syntax doesn't have any `keyword` in it, it doesn't work at `Package Scope`.
- For e.g.
    ```go
    package main

    main() {
        someFlag := true // 'var' keyword and 'variable type' is not specified. It works!
    }
    ```

----------------------------------------

```diff
- Multiple Short Declarations
```
- We can declare and initialize `multiple variables` of `different types` using `short declaration` syntax:
    ```go
    package main

    main() {
        someFlag, age, name := true, 30, "आदित्य" // Multiple variables of different types.
    }
    ```
- In this type of declaration, number of values and number of names must be the same. Otherwise it will result in error.

----------------------------------------

```diff
- Redeclarations With Short Declarations
```
- `Short Declaration` can initialize new variables and assign to existing variables at the same time.
- **At least one of the variable in `Short Declaration Redeclaration` must be a new variable**.
- For e.g.
    ```go
    package main

    main() {
        var someFlag bool

        // someFlag := true // Error! At least one variable must be new to make this work.
        someFlag, age := true, 30 // This works! Because 'age' is a new variable being declared in the same statement. someFlag will be set (redeclared) to true.
    }
    ```

----------------------------------------

```diff
+ Blank Identifier
```
> “There are only two hard things in Computer Science: cache invalidation and naming things”. Tim Bray quoting Phil Karlton
- Go doesn't allow `unused variables` in `blocked scope`.
- To ignore a variable, `Blank Identifier (_)` is used as a variable name in Go.
- Go compiler will not throw unsed variable error if a blocked scope variable is named `_`.
- We cannot use value assigned to `_`.
- It is like a black hole that swallows variable.
- [Detailed information and usage of Blank Identifier](https://golang.org/doc/effective_go.html#blank)

----------------------------------------

```diff
+ fmt.Printf and fmt.Sprintf Formatting
```
- Following formatting can be used with `fmt.Printf` as well as `fmt.Sprintf`:
    ```go
    // String and slice of bytes
    %s  // the uninte­rpreted bytes of the string or slice
    %q  // a double­-quoted string safely escaped with Go syntax
    %x  // base 16, lower-­case, two characters per byte
    %X  // base 16, upper-­case, two characters per byte

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // Boolean
    %t  // the word true or false

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // General
    %v  // The value in a default format. When printing structs, the plus flag (%+v) adds field names.
    %#v // a Go-syntax repres­ent­ation of the value
    %T  // a Go-syntax repres­ent­ation of the type of the value
    %%  // a literal percent sign; consumes no value

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // Integer
    %b  // base 2
    %c  // the character repres­ented by the corres­ponding Unicode code point
    %d  // base 10
    %o  // base 8
    %q  // a single­-quoted character literal safely escaped with Go syntax
    %x  // base 16, with lower-case letters for a-f
    %X  // base 16, with upper-case letters for A-F
    %U  // Unicode format: U+1234; same as "­U+%­04X­"

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // The default format for %v
    bool // %t
    int, int8 // %d
    uint, uint8 // %d, %x if printed with %#v
    float32, complex64 // %g
    string // %s
    chan // %p
    pointer // %p

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // Floati­ng-­point and complex consti­tuents
    %b  // decima­lless scientific notation with exponent a power of two, in the manner of strcon­v.F­orm­atFloat with the 'b' format, e.g. -12345­6p-78
    %e  // scientific notation, e.g. -1.234­456e+78
    %E  // scientific notation, e.g. -1.234­456E+78
    %f  // decimal point but no exponent, e.g. 123.456
    %F  // synonym for %f
    %g  // %e for large exponents, %f otherwise
    %G  // %E for large exponents, %F otherwise

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // Floati­ng-­point Precision
    %f      // default width, default precision
    %9f     // width 9, default precision
    %.2f    // default width, precision 2
    %9.2f   // width 9, precision 2
    %9.f    // width 9, precision 0

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // Pointer
    %p  // base 16 notation, with leading 0x

    // +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

    // Other flags
    +   // always print a sign for numeric values; guarantee ASCII-only output for %q (%+q).
    -   // pad with spaces on the right rather than the left (left-­justify the field).
    #   // alternate format: add leading 0 for octal (%#o), 0x for hex (%#x); 0X for hex (%#X); suppress 0x for %p (%#p); for %q, print a raw (backq­uoted) string if strcon­v.C­anB­ack­quote returns true;
    ' ' (space)  // leave a space for elided sign in numbers (% d); put spaces between bytes printing strings or slices in hex (% x, % X).
    0   // pad with leading zeros rather than spaces; for numbers, this moves the padding after the sign.
    ```

----------------------------------------

```diff
+ Slice Vs. Array - Performance
```
- **`Slice` operations are cheap!**
- `Slicing`: Creates a new `slice header`.
- `Assigning a Slice to another Slice` or, `passing it to a function`: Only copies the `slice header`.
- `Slice header has a fixed size` and `it doesn't change` even if we have got millions of elements.
- **`Array` can be expensive as compared to `Slice`**.
- `Assigning an array to another array` or `passing it to a function`: Copies all the elements of it.

----------------------------------------

```diff
+ Composite Types In Go
```
- Following are the `Composite Types` in Go:
    * **Arrays**: Collection of elements. `Indexable` and `Fixed Length`.
    * **Slices**: Collection of elements. `Indexable` and `Dynamic Length`.
    * **Strings**: Byte Slices. ASCII and UNICODE.
    * **Maps**: Collection of `Indexable Key-Value Pairs`.
    * **Structs**: Groups different types of variables together.

----------------------------------------

## Type System In Go
- At `compile time`, a Go `compiler` can catch `overflow errors`.
- In runtime, when `overflows` occurs:
    * `integer` wrap arounds and go to their minimum and maximum values.
    * `float` wrap arounds to `positive infinity` or `negative infinity`.

----------------------------------------

```diff
+ Important Links
```
- [What is Binary?](https://simplicable.com/new/binary)
- [What's a Bit](https://en.wikipedia.org/wiki/Bit)
- [What's a Byte](https://en.wikipedia.org/wiki/Byte)
- [How old school graphics worked? PART 1](https://www.youtube.com/watch?v=Tfh0ytz8S0k)
- [How old school graphics worked? PART 2](https://www.youtube.com/watch?v=_rsycfDliZU)
- [Stackoverflow: What actually is a Byte Stream?](https://softwareengineering.stackexchange.com/questions/216597/what-is-a-byte-stream-actually/216600#216600)
- [Why Byte but not uint8?](https://github.com/golang/go/issues/22180)

----------------------------------------

```diff
+ Predeclared Types
```
- A `predeclared type` is a `built-in type` that we can use everywhere without `importing` any `package`.
- A `built-in type` means it's a `core feature of Go` i.e. it comes with `compiler` itself.
- A `predeclared type` has a name and we can use it in `any scope`.
- We don't have to `declare` a `predeclared type` before using it.
- It has a `type representation` i.e. how Go see it and how we can use it. In other words, `what values a type can represent`.
- It has a `size in bytes` i.e. how much space it needs in memory and it also determines the range of values it can represent.
- Go cannot catch `overflow errors` in `runtime`. For e.g. A `variable` belongs to `runtime` and it's value cannot be known at the `compile time`.
- In Go, when variable values `overflow`, they gets wrapped around i.e. They get reassigned to the minimum value their `variable type` can represent.
- Examples of `Predeclared Types`:
    ```go
    bool // 'bool' is a predeclared type and it has following characteristics:
    // Name: bool
    // Representation: 'true' or 'false'
    // Size: 1 byte

    int // 'int' is a predeclared type and it has following characteristics:
    // Name: int
    // Representation: -1, 0, 1, 1000000000000000
    // Size: 8 byte
    ```

----------------------------------------

```diff
+ Defined Types
```
- **A `Defined Type` is also called as `Named Type`.**
- A `Defined Type` can only be created from another existing `Type`.
- We need to give a new name to newly created type.
- Newly `Defined Type` can optionally have it's own `methods`.
- A `type` can be `converted` to `another type` if they `share` the `same underlying type` and vice versa.
- A `defined type` and it's `source type` share the same `underlying type`.
- For e.g.
    ```go
    // 'Duration' is the 'defined type' or 'named type'.
    // 'int64' is the 'underlying type'.
    type Duration int64

    // Type conversion
    var microSeconds int64   // 'microSeconds' variable is of type 'int64'
    var nanoSeconds Duration // 'nanoSeconds' variable is of type 'Duration'

    nanoSeconds = microSeconds // ERROR! This won't work. To make it work:
    nanoSeconds = Duration(microSeconds) // Works! We are converting 'microSeconds' to 'Named Type' we have created above i.e. 'Duration'
    microSeconds = int64(nanoSeconds) // This also works!
    ```

----------------------------------------

```diff
+ Aliased Types
```
- `byte` and `uint8` are exactly `the same types` just with `diferent names`.
- `rune` and `int32` are exactly `the same types` just with `diferent names`. i.e. `rune` is an `alias` of `int32`. The `rune` type is used to represent `unicode characters`.
- `Type Alias` declaration is not for everyday usage. It is mainly used in very huge codebase refactors.

----------------------------------------

## Constants
- `Constants` belong to `compile time`. They must be initilized with value when they are declared.
- `Constants` are created at `compile time`. In the `run time`, Go just transforms it into a `value`.
- **Unnamed constants**: All `basic literals` are `unnamed constants`. Following are examples of `basic literals`:
    ```go
    // Unnamed constants
    1
    3.14
    "hello"
    true
    false
    ```
- **Named Constants**: All `named constants` will be replaced to their `values` in `runtime`. They need to be `declared` first.
- **Untyped Constants**: `Constants` may or may not have a type.
- If the value is'nt going to change throughout our program's lifetime and we already know the value (if it belongs to compile time) then we should go for `named constants`.
- Constants are `immutable` i.e. we cannot change their values.
- We `cannot initialize` a constant to a `runtime value`.
- We can use `expressions` while initializing `constants`.

----------------------------------------

```diff
+ Important Links
```
- [Go’s typed and untyped constants](https://blog.learngoprogramming.com/learn-golang-typed-untyped-constants-70b4df443b61)
- [Go enums and iota — Full of tips and tricks with runnable code examples](https://blog.learngoprogramming.com/golang-const-type-enums-iota-bc4befd096d3)

----------------------------------------

```diff
+ Constant Types
```
- We can declare constants using `non-numeric` types as well.
- Constants don't have to be only `numeric values`.
- We `don't have to` declare the `types` of constants.
- For e.g.
    ```go
    func main() {
        // Below works..
        const min       int     = 1
        const pi        float64 = 3.14
        const version   string  = "2.0.3"
        const debug     bool    = true

        // Declaring constants without types also works.
        const min     = 1
        const pi      = 3.14
        const version = "2.0.3"
        const debug   = true

        // We can use expressions while initializing constants.
        const min     = 1 + 1               //2
        const pi      = 3.14 * min          // 6.28
        const version = "2.0.3" + "-beta"   // 2.0.3-beta
        const debug   = !true               // false
    }
    ```

----------------------------------------

```diff
+ Multiple Constants Declaration
```
- `Constants` get their `types` and `expressions` from `the previous constant`.
- We can declare `multiple constants` in a single go as below:
    ```go
    func main() {
        // Multiple constants of same type in one go
        const min, max int = 1, 1000

        // Declaring in group
        const (
            min int = 1
            max int = 1000
        )

        // Constants get their types and expressions from the previous constant
        const (
            min int = 1000     // 1000
            max                // 1000
        )
    }
    ```

----------------------------------------

```diff
+ Typeless Or Untyped Constants
```
- When we declare `a constant without a type`, it becomes `untyped constant (typeless constant)`.
- All `basic literals` are also `typeless`. They all are `typeless constant values`.
- A `constant with a type` can only be used with `a value` of `the same type`.
- The `untyped numeric constant` can be used with `all numeric values` together.
- For e.g.
    ```go
    func main() {
        const min = 42

        var i int      =  min  // Type of constant 'min' = int
        var f float64  =  min  // Type of constant 'min' = float64
        var b byte     =  min  // Type of constant 'min' = byte
        var j int32    =  min  // Type of constant 'min' = int32
        var r rune     =  min  // Type of constant 'min' = rune
    }
    ```

----------------------------------------

```diff
+ Default Types
```
- **`Conversion` only happens when `a type is needed`.**
- Go `converts` a `typeless constant` to a `typed` value when a `type` is needed.
- For e.g.
    ```go
    func main() {
        const min int32 = 1000

        max := 5 + min // Type of 'max' is 'int32'
                       // Internally this happens: max := int32(5) + min
    }
    ```
- Go `implicitly converts` the `typeless constant` to a `typed value`.
- For e.g.
    ```go
    func main() {
        const min = 1000

        max := 5 + min // Type of 'max' is 'int'
                       // Internally this happens: max := int(5) + int(min)
    }
    ```
- An `untyped constant` has a `default type`.
- Go `evaluates the expression` then it `converts` the `resulting typeless value` to its `default value`.

----------------------------------------

```diff
+ IOTA
```
- `IOTA` is nothing but a `number generator` for `constants`. In other words, it is ever increasing automatic counter.
- `IOTA` is built-in `constant generator` which `generates` ever `increasing numbers`.
- `IOTA` starts at `0`.
- We can use `expressions` with `IOTA`. So, the other `constants` will `repeat` the `expressions`.
- We can use blank identifier (_) to adjust the values of constants:
    ```go
    func main() {
        const (
            EST = -(5 + iota)   // -5
            _                   // -6 | Discarded/skipped due to blank identifier
            MST                 // -7
            PST                 // -8
        )
    }
    ```

----------------------------------------

```diff
+ Common Abbreviations Used In Go
```
- Following are some of the common Abbreviations used in Go standard libraries:
    ```go
    var s string        // string
    var i int           // index
    var num int         // number
    var msg string      // message
    var v string        // value
    var val string      // value
    var fv string       // flag value
    var err error       // error value
    var args []string   // arguments
    var seen bool       // has seen?
    var parsed bool     // parsing ok?
    var buf []byte      // buffer
    var off int         // offset
    var op int          // operation
    var opRead int      // read operation
    var m int           // another number
    var c int           // capacity
    var c int           // character
    var sep string      // separator
    var src int         // source
    var dst int         // destination
    var b byte          // byte
    var b []byte        // buffer
    var buf []byte      // buffer
    var w io.Writer     // writer
    var r io.Reader     // reader
    var pos int         // position

    // ...list goes on and on...
    ```
- Use the complete words in larger scopes such as `package scope`.
- Use `mixedCaps`.
- Use all capital letters for common acronyms such as `API`.
- Do not use `underscores` in names.

----------------------------------------

## Error Handling
- In Go, `nil` value is extensively used for Error Handling.
- For e.g.
    ```go
    func main() {
        data, err := someFunc()

        if err != nil {
            fmt.Println("Error occurred")
            return
        } else {
            fmt.Println("Success")
        }
    }
    ```

----------------------------------------

```diff
+ nil
```
- `nil` is a `predeclared identifier` like `true`, `false`, `len()`, `int32`, `float64` etc.
- Since it is a `predeclared identifier`, it can be used anywhere `without importing` any `package`.
- `nil value` means that the value is `not initialized` yet.
- It is similar to following identifiers in other languages:
    ```javascript
    null    // JavaScript
    None    // Python
    null    // Java
    nil     // Ruby
    ```
- The `zero value` of all `pointer-based` types in Go is `nil`. Following are the `pointer-based` types in Go:
    ```javascript
    pointers
    slices
    maps
    interfaces
    channels
    ```
- In Go, `nil` value can be `untyped` or `typed` depending on the `context`.

----------------------------------------

## Strings Runes And Bytes
```diff
+ Important Links
```
- [Representing letters with numbers - Overview of ASCII and Unicode](https://youtu.be/1GSjbWt0c9M?t=403)
- [Characters in a computer - Advanced technical videos about the underlyings of ASCII and Unicode](https://www.youtube.com/watch?v=B1Sf1IhA0j4&list=PLhQN_EIoIKBRA0yVTsWDoJzEKZwJY0p3l)
    * **The 3rd video is especially important because it talks about UTF-8 encoding and decoding.**
- [Hexadecimal Number System - Hexadecimal numbers are important when working with bytes](https://www.youtube.com/watch?v=4EJay-6Bioo)
- [Go Blog: Strings](https://blog.golang.org/strings)

----------------------------------------

```diff
+ Strings Runes And Bytes 101
```
- A `string value` is nothing but a `series of bytes`.
- We can represent a `string value` as a `byte slice`. For e.g.
    ```go
    "hey"                   // String value
    []byte{104, 101, 121}   // Representing string "hey" in byte slice

    []byte("hey")                   // Converting string "hey" into byte slice
    string([]byte{104, 101, 121})   // Converting byte slice into string value
    ```
- Instead of `numbers` (byte slice), we can also represent `string characters` as `rune literals`.
- `Numbers` and `Rune Literals` are the same thing.
- **In Go, `Unicode Code Points` are called `Runes`.**
- A `Rune literal` is a `typeless integer literal`.
- A `Rune literal` can be of `any integer type`. for e.g. `byte (uint8)`, `rune (int32)` or `any other integer type`.
- **In short, `Rune` is a `Unicode Code Point` that is represented by an `Integer Value`.**
- Using `UTF-8` we can represent `Unicode Code Points` between `1 byte` and `4 bytes`.
- We can represent any `Unicode Code Point` using the `Rune Type` because it can store `4 bytes` of data. For e.g.
    ```go
    char := '🍺'
    ```
- `String values` are `read-only byte slices` i.e.
    ```go
    string value ----> read-only []byte
    ```
- `String to Byte Slice` conversion creates a `new []byte slice` and **copies** the bytes of the string to a new slice's `backing array`. They don't share the same `backing array`.
- In short, `String` is an `immutable byte slice` and we cannot change any of it's elements. However, we can convert `string to a byte slice` and then we can change that `new slice`.
- **A `string` is a data structure that points to a `read-only backing array`.**
- `UTF-8` is a `variable length encoding` (for efficiency). So each `rune` may start at a `different index`.
- `for range` loop jumps over the `runes of a string`, rather than the `bytes of a string`. Each `index` returns the `starting index` of the `next rune`.
- `Runes` in a `UTF-8 encoded string` can have a different number of `bytes` because `UTF-8` is a `variable byte-length encoding`.
- Especially in scripting languages, we can manipulate` UTF-8 strings` by `indexes` easily. However, Go doesn't allow us to do so `by default` because of **efficiency reasons**.
- Go never hides the cost of doing something.
- `[]rune(string)` creates a `new slice`, and **copies** each `rune` to new slice's `backing array`. **This is inefficient way of indexing strings.**
- A `string` value usually use `UTF-8` so it can be **more efficient** because each `rune` on the other hand `uses 1 to 4 bytes` (variable-byte length).
- Each `rune` in `[]rune` (Rune Slice) has the same length i.e. `4 bytes`. It is **inefficient** because the `rune` type is an **alias** to `int32`.
- **In Go, if our `source code file` is encoded into `utf-8` then `String Literals` in our file are automatically encoded into `utf-8`.**
- When we're working with `bytes`, continue working with `bytes`. Do not convert a `string` to `[]byte` (Byte Slice) or vice versa, unless necessary. **Prefer working with `[]byte` (Byte Slice) whenever possible.** `Bytes` are more efficient and used almost everywhere in Go standard libraries.

----------------------------------------

## Maps In Go
```diff
+ Maps 101
```
- `Maps` allows us to quickly access to an `element/value `using a `unique key`.
- `Map keys` must be `unique` because otherwise it can't find the corresponding `values/elements`.
- The types of `Map Keys` and `Values in Maps` can be `different`.
- A `Map Key` must be a `comparable type`.
- All `Map Keys` and `Map Values` must `belong` to their `corresponding types`. They `can't be mixed up`.
- A `Map Variable (or a Value)` is nothing but a `pointer` to a `Map Header Value` in the `memory`.
- A `Map Value` only contains the `memory address` of a `Map Header`.

----------------------------------------

## Structs In Go
```diff
+ Inheritance vs. Composition
```
![Inheritance vs. Composition](https://turnoff.us/image/en/cokorange.png)

----------------------------------------

```diff
+ Structs 101
```
- `Structs` are `blueprints` — They are `fixed` at `compile-time`.
- It's like a `class` in `OOP` languages. Groups `related data` in a `single type`.
- `Struct types` are created at `compile-time`.
- A `struct` may store `different types` of data.
- `Struct fields` are declared at `compile-time`. However, `struct values` fill them in `runtime`.
- The `field names` and `types` are declared at `compile-time`. They are `fixed` and cannot change in `runtime`.
- `Field values` belong to `runtime`. We can change them in `runtime`.
- `Structs` cannot dynamically `grow` but they can have `different` set of `types`.
- Struct example:
    ```go
    type VideoGame struct {
        Title string Genre string
        Published bool
    }
    ```
- Two `structs` are `equal` if all their `fields` are `equal`.
- `Anonymous Fields`: When the `field names conflict` the` parent type` takes `priority`.

----------------------------------------

## OOP In Go With Methods And Interfaces
```diff
+ Methods
```
- `Methods` enhance `types` with additional behavior.
- `Methods` of the `type` are called `Method Set`.
- To attach method to a `type`:
    ```go
    // Syntax
    // "varName Type" is called a "receiver"
    func (varName Type) funcName() {
        // Code
    }

    // Example
    // "book" is a struct here
    func (b book) printBook() {
        fmt.Println(b.title, b.price)
    }
    ```
- A `receiver` is nothing but method's `input parameters` written `before` a `method name`.
- A `method` belongs to a `single type`.
- `Methods` on `different types` can have the `same names`.
- `Method Expressions` allows us to call `methods` through `types`. For e.g.
    ```go
    // "game" is a struct type
    game.print(cod)
    game.print(battlefield)
    ```
- Behind the scenes, a `method` is a `function` that takes `receiver` as it's `first argument`.

----------------------------------------

```diff
+ Pointer Receivers
```
- We can define `methods` on `types` using `Pointer Receivers`.
- The only difference between `method` and a `function` is that a `method` belongs to a `type`, whereas a `function` belongs to a `package`.
- **Consistent Design Tip**: When one of the `methods` in any `type` are using `pointer receiver`, it is better to convert all `method receivers` of that `type` to `pointer receivers`.
- **We (must) use a `pointer receiver` when we want to make changes to a `receiver variable`. In other words, use a `pointer receiver` when the received value into `method` is going to be very large.**

----------------------------------------

```diff
+ Attaching Methods To Any Types
```
- We can attach methods to any type in Go. For e.g.
    ```go
    // Basic Types
    int
    string
    float64

    // Bare Types
    array
    struct

    // -----------------------
    // Do not use "Pointer Receivers" with below types since they already carry a pointer with themselves.
    // i.e. slice, map, chan, func
    // -----------------------
    // Pointer Bearing Types
    slice
    map
    chan // Channels

    // We can also attach methods to:
    func
    ```

----------------------------------------

```diff
+ Interfaces
```
- We declare an `Interface` much like as we define a `user defined type`.
- `Interfaces` decouple different `types` from each other so we can create more maintainable programs.
- An `Interface` is a `Protocol`, a `Contract`.
- **Bigger the `Interface` the weaker the `abstraction`. --> `Rob Pike`**
- It's an `abstract type`. It doesn't have any implementation. It only describes `the expected behavior`.
- The opposite of `Abstract Type` is `Concrete Type`.
- All the `types` in Go except `Interface` are of `Concrete Type`.
- For e.g. Following are `Concrete Types`:
    ```go
    // Concrete Types
    int
    string
    float64
    array
    struct
    slice
    map
    chan
    func
    ```
- **The Interface only defines the expected behavior.**
- Go does not have an implements keyword.
- `A Type satisfies an Interface automatically` when it has `all the methods of the Interface` without explicitely specifying it.
- `Interface` values are `comparable`.
- `Go interfaces` are `implicit`. **The implementing types don't need to specify that they implement an interface.**
- Interface declaration example:
    ```go
    type MyInterface interface {
        foo() int
        bar() float64
        baz() string
    }
    ```

----------------------------------------

```diff
+ Type Assertion
```
- `Type Assertion` allows us to `extract` the `dynamic value` from `Interface`.
- It can also be used to check (assert) whether the `Interface Value` provides the `method` we want.

----------------------------------------

```diff
+ Empty Interface
```
- **Do not use `Empty Interface` unless really necessary.**
- Every `type` in Go implements the `empty interface`.
- An `Interface Value` has 2 parts:
    * A dynamic `Value`.
    * A dynamic `Type`.
- `Empty Interface` is the one which doesn't have any `methods`.
- Every `Type` satisfies the `Empty Interface`.
- It can represent any `Type` of `Value`.
- We can't directly use the `dynamic value` of an `empty interface value`.
- Example of Empty Interface:
    ```go
    type someInterface interface {

    }
    ```
- To use a `value` from `Empty Interface`, we first need to `extract` it using `Type Assertion`.
- `Empty Interface Slice` contains the `Empty Interface Values`.
- Example use cases of empty interfaces:
    * A function that returns a value of `interface{}` can return **any** `type`.
    * We can store heterogeneous values in an `array`, `slice`, or `map` using the empty `interface{} type`.

----------------------------------------

```diff
+ Type Switch
```
- `Type Switch` allows us to detect and extract `dynamic values` from `Interface Values` using `Switch Statement`.
- When we have lots of conditions to check, we can use `Type Switch`.
- Example of `Type Switch` statement:
    ```go
    // "v"     ---> Interface Value
    // "type"  ---> Extracts type from the Interface Value "v"
    // "e"     ---> Extracted value will be assigned to variable "e". It changes depending on the extracted value.
    switch e := v.(type) {
        case int:
            // "e" is an "int" here..
        case string:
            // "e" is an "string" here..
        default:
            // "e"'s type equals to "v"'s type..
    }
    ```
- Unlike regular `Switch` which compares `values`, the `Type Switch` compares `types of the values`.

----------------------------------------

## Concurrency And Parallelism
```diff
+ Concurrency
```
- The composition of independently executing tasks.
- Applied when dealing with handling lots of things at once.
- The focus is on how to structure a solution to solve a problem which may or may not be solved in a parallel manner.
- `Concurrency` is a way to structure a program by breaking it into pieces that can be executed independently.
- Communication is the means to coordinate the independent executions.
- Go supports concurrency. Go provides:
    * concurrent execution (`goroutines`).
    * synchronization and messaging (`channels`).
    * multi-way concurrent control (`select`).
- **IMPORTANT POINTS:**
    * `Concurrency` is powerful.
    * `Concurrency` is not `parallelism`.
    * `Concurrency` enables `parallelism`.
    * `Concurrency` makes `parallelism` (and scaling and everything else) easy.

----------------------------------------

```diff
+ Parallelism
```
- `Parallelism` is the simultaneous execution of computations.
- Programming as the simultaneous execution of (possibly related) computations.
- It's all about doing lots of things at once.

----------------------------------------

```diff
+ Concurrency vs. parallelism
```
- **“Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once.” — Rob Pike**
    * **Concurrency is a property of a program where two or more tasks can be in progress simultaneously. Parallelism is a run-time property where two or more tasks are being executed simultaneously. Through concurrency you want to define a proper structure to your program. Concurrency can use parallelism for getting its job done but remember parallelism is not the ultimate goal of concurrency.**
- `Concurrency` is about dealing with lots of things at once.
- `Parallelism` is about doing lots of things at once.
- Not the same, but related.
- `Concurrency` is about `structure`, `parallelism` is about `execution`.
- `Concurrency` provides a way to `structure a solution` to solve a problem that may (but not necessarily) be `parallelizable`.
- An analogy
    * `Concurrent`: Mouse, keyboard, display, and disk drivers.
    * `Parallel`: Vector dot product.

----------------------------------------

## Goroutines
- In Go, `concurrency` is achieved by using `Goroutines`.
- `Goroutines` are `functions` or `methods` which can run `concurrently` with others `methods` and `functions`.
- `Goroutines` are lightweight `threads` that are managed by the `Go runtime`.
- They are very much similar like `threads` in `Java` but light weight and cost of creating them is very low.
- When we run a function as a `Goroutine`, we are running the function `concurrently`.
- Place the keyword `go` before a function call to execute it as a `Goroutine`.
- To run a method or function concurrently prefix it with keyword `go`.
- For e.g.
    ```go
    package main

    import (
        "fmt"
        "time"
    )

    func print() {
        fmt.Println("Printing from goroutine")
    }

    func main() {
        go print()
        time.Sleep(1 * time.Second)

        fmt.Println("Printing from main")
    }
    ```
- Program is terminated when `main()` function execution is completed. When the program terminates, all `Goroutines` are terminated regardless of the fact if all the `Goroutines` has completed execution or not.
- We can also run `Anonymous Functions` as `Goroutines` as follows:
    ```go
    // Executing anonymous function as Goroutine
    go func() {
        //
    }()
    ```

----------------------------------------

```diff
+ Advantages of Goroutines over Threads
```
- `Goroutines` have a faster startup time than `threads`.
- `Goroutines` come with `built-in primitives` to communicate safely between themselves called as `channels`.
- `Goroutines` are extremely cheap when compared to `threads`. They are only a few `kb` in `stack size` and the `stack` can grow and shrink according to needs of the application whereas in the case of `threads` the `stack size` has to be specified and is `fixed`.

----------------------------------------

## Channels
- `Channels` are conduits (pipes) that we can use to pass values of a particular `type` from one `Goroutine` to another.
- `Channels` are a mechanism for communication.
- `Channels` allows `Goroutines` to share memory by communicating
- We can use `Channel Operators: <-, ->` to send and receive values.
    * **NOTE:** The data flows in the direction of the `arrow`.
- We can create a `Channel` using built-in `make()` function as below:
    ```go
    ch := make(chan type) // type: Data Type
    ```
- **`Normal Channels` are `Synchronous`.** i.e. Both the sending side and the receiving side of the `channel` **wait** until the other side is ready.

----------------------------------------

```diff
+ Buffered Channels
```
- **`Buffered Channels` are `Asynchronous`.** i.e. Sending and Receiving messages through `Buffered Channels` will not block unless the `Channel` is full.
- We can create a `Buffered Channel` same way as we create the `Normal Channels` using the built-in `make()` function. The only difference is, we can pass the second parameter to `make()` function which indicates the Buffered Channel's `Buffering Capacity`.
- For e.g.
    ```go
    ch := make(chan type, capacity)
    ```
- **NOTE: If we pass the `Buffering Capacity` as `1`, we are creating a `Normal Channel`. To create a `Buffered Channel`, we have to pass `Buffering Capacity` as `greater than 1`**

----------------------------------------

## Mutexes And Wait Groups From GoSync Package
- Since `Goroutines` run in a `same address space`, they have access to `shared memory` and this `access` must be `synchronised`. Go's motto is to `share memory` by `communicating` (`Goroutines` and `Channels` makes this possible).
- Sometimes, some problems are better suited to using the `traditional forms of synchronisation`. Go allows us to make use of these `Synchonisation Primitives` by using the `Sync` package.

----------------------------------------

```diff
+ Mutexes
```
- **A `Race Condition` happens when two or more `threads` can access `shared data` and try to change that `shared data` at the same time. We can use `Mutex` to solve this problem.**
- A `Mutex` is a `Mutual Exclusion Lock`. It's a `Synchronisation Primitive`.
- It is used to `protect shared data` which is simultaneously `accessed` by `multiple treads`.
- Making changes to shared data and reading a shared data (e.g. for printing purposes) are still considered accessing the same data simultaneously.

----------------------------------------

```diff
+ Wait Groups
```
- `Wait Groups` are another `Synchronisation Primitive`.
- A `Wait Group` basically waits for `collection` of `Goroutines` to finish execution.

----------------------------------------

## Go Vet
- `go vet` command helps us catch errors which are not generally caught by Go Compiler.
- For e.g.
    * Go to directory: `62-Go-Vet-To-Catch-Errors`
    * Execute file with below command:
        ```sh
        go vet main.go
        ```
    * It will catch the error where `int` is supplied to `fmt.Printf()` whereas `string` value should've been supplied. This error isn't caught by Go Compiler since the program is still syntactically correct.

----------------------------------------

## Go Documentation Server On Local Machine
- To start a Go Documentation Server On Local Machine, execute:
    ```sh
    # Install godoc with following command first:
    # go get golang.org/x/tools/cmd/godoc

    # Then execute:
    godoc -http=:6060
    ```
- In browser, visit:
    ```
    http://localhost:6060
    ```

----------------------------------------
