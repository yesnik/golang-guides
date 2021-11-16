# Interfaces

An interface type is defined as a set of method signatures.

A type implements an interface *by implementing its methods*. There is *no explicit declaration of intent*, no "implements" keyword.

Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

```go
type Questionable interface {
	AddQuestionMark() string
}

type Word struct {
	W string
}

// This method means type Word implements the interface Questionable,
// but we don't need to explicitly declare that it does so.
func (w Word) AddQuestionMark() string {
	return w.W + "?"
}

func main() {
  // A value of interface type can hold any value that implements those methods
	var leo Questionable = Word{"leo"}
	fmt.Println(leo.AddQuestionMark()) // leo?
}
```
