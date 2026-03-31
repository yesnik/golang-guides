# text/template

Package [text/template](https://pkg.go.dev/text/template) implements data-driven templates for generating textual output.

To insert data in a template, you add **actions** to the template text. 
Actions are denoted with double curly braces, `{{ }}`. 
Inside the double braces, you specify data you want to insert or an operation you want the template to perform. 
Whenever the template encounters an action, it will evaluate its contents, and insert the result into the template text in place of the action.

Within an action, you can reference the data value that was passed to the `Execute` method with a single period, called "dot".

```go
import (
	"log"
	"os"
	"text/template"
)

func check(err error) {
	if err != nil {
		log.Fatal(err)
	}
}

func executeTemplate(text string, data interface{}) {
	templ, err := template.New("test").Parse(text)
	check(err)
	err = templ.Execute(os.Stdout, data)
	check(err)
}

func main() {
	executeTemplate("Dot is: {{.}}\n", "ABC") // Dot is: ABC
	executeTemplate("start {{if .}}Dot is true!{{end}}\n", true) // start Dot is true!
	executeTemplate("start {{if .}}Dot is true!{{end}}\n", false) // start

	templateText := "Before loop: {{.}}\n{{range .}}In loop: {{.}}\n{{end}}After loop: {{.}}\n"
	executeTemplate(templateText, []string{"do", "re", "mi"})
	/*
	Before loop: [do re mi]
	In loop: do
	In loop: re
	In loop: mi
	After loop: [do re mi]
	*/

	type Student struct {
		Name string
		Age int
	}
	templateText = "Name: {{.Name}}, Age: {{.Age}}"
	executeTemplate(templateText, Student{Name: "Kenny", Age: 18}) // Name: Kenny, Age: 18
}
```
