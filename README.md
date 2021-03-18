# golang-as-proposal
Proposal for GO error handling with `as` syntax. This proposal is on brainstorming stage.

## Example 1 - function with one return

```go
func f1() (error){
  //...
}

func run1() error {
  f1() as err {
    // this block will only be executed if `err != nil`
    return err
  }
}
```

## Example 2 - function with two return
```go

type Book struct{}

func f2() (Book, error) {
  //... some blocks return `nil, errors.New("error")
  return Book{}, nil
}

func run2() error {
  b := f2() as err {
    // this block will only be executed if `err != nil`
    return err
  }
  
  fmt.Printf("%v", b)
}
```

## Example 3 - use `try as` instead of `as` to indicate it will assert if the value of `err` is nil or not
```go

type Book struct{}

func f3() (Book, error) {
  //... some blocks return `nil, errors.New("error")
  return Book{}, nil
}

func run3() error {
  b := f2() try as err {
    // this block will only be executed if `err != nil`
    return err
  }
}
```
