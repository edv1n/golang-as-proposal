# GO (Golang) `as` Syntax Proposal
Proposal for GO error handling with `as` syntax. This proposal is on brainstorming stage.

## Syntax Definition

Assume we have a function `f()`
```go
func f() (var1, ...varN) { ... }
```

To use the `as` syntax
```go
var1, ...varSecondLast := f() as varN {
  // varN available here
  //... code block
}

// var1, ...varSecondLast available here
// varN not available here
```

The `as` keyword indicats any results not assigned to LHS will be assigned to variable defined after `as`. Currently, the number of variable definable after `as` is limited to one only, but this can be relaxed. The code block defined after `as` will only be executed if the value assigned is not `nil`.

## Example

### Example 1 - function with one return (error handling)

```go
func f1() error {
  //...
}

func run1() error {
  f1() as err {
    // this block will only be executed if `err != nil`
    // `err` is scopped in this block only
    return err
  }
  
  fmt.Printf("%v", err) //compile error, as `err` is scoped in earlier block
  return nil
}
```

This is only useful if we want to execute `f1()` and handle the return value in a scoped block.

### Example 2 - function with two returns
```go

type Book struct{}

func f2() (*Book, error) {
  //... some blocks return `nil, errors.New("error")
  return Book{}, nil
}

func run2() error {
  b := f2() as err {
    // this block will only be executed if `err != nil`
    // `err` is scopped in this block only
    return err
  }
  
  fmt.Printf("%v", b) // print the content of b
  fmt.Printf("%v", err) //compile error, as `err` is scoped in earlier block
}
```

In this case, `b` will contain the first result returned by `f2()`. Since the second returned value is not assigned to LHS, it will be passed to the `as` block. The second value will be assigned to `err`.

### Example 3 - function with one return (non error related handling)

```go
func f1() *time.Time {
  //...
}

func run1() {
  f1() as t {
    // this block will only be executed if `t != nil`
    // `t` is scopped in this block only
    fmt.Printf("%v", t)
  }
  
  fmt.Printf("%v", t) //compile error, as `t` is scoped in earlier block
  return nil
}
```

I am not sure how useful will this be. But I keep this example as one of the use cases.

### Example X - use `try as` instead of `as` to indicate it will assert if the value assigned to RHS is nil or not
```go

type Book struct{}

func f3() (Book, error) {
  //... some blocks return `nil, errors.New("error")
  return Book{}, nil
}

func run3() error {
  b := try f2() as err {
    // this block will only be executed if `err != nil`
    return err
  }
}
```
