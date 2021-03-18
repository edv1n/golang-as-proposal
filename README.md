# golang-as-proposal
Proposal for GO error handling with `as` syntax. This proposal is on brainstorming stage.

## Example 1 - function with one return

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

## Example 2 - function with two returns
```go

type Book struct{}

func f2() (Book, error) {
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

In this case, `b` will be a value with type `Book`. Since the second returned value is not assigned to LHS, it will be passed to the `as` block. The second value will be assigned to `err`.

## Example X - use `try as` instead of `as` to indicate it will assert if the value of `err` is nil or not
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
