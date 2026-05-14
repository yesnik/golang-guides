# Database

## Sql

Package [sql](https://pkg.go.dev/database/sql) provides a generic interface around SQL (or SQL-like) databases. The `sql` package must be used in conjunction with a database driver.

```go
package main

import (
	"database/sql"
	_ "github.com/go-sql-driver/mysql" // Blank import to register the driver
)

func main() {
	// Standard DSN example
	dsn := "user:pass@tcp(127.0.0.1:3306)/mydbname?parseTime=true"

	db, err := sql.Open("mysql", dsn)
	if err != nil {
		panic(err)
	}
	if err = db.Ping(); err != nil {
		panic(err)
	}

	defer db.Close()
}
```

- Notice how the import path for our driver is prefixed with an underscore?
  This is because our `main.go` file doesn't actually use anything in the mysql package.
  So if we try to import it normally the Go compiler will raise an error. However, we need the driver's `init()`
  function to run so that it can register itself with the `database/sql` package. The trick to
  getting around this is to alias the package name to the *blank identifier*.
- The `sql.Open()` function doesn't actually create any connections, all it does is initialize the pool for future use.
  Actual connections to the database are established lazily, as and when needed for the first time.
  So to verify that everything is set up correctly we need to use the `db.Ping()` method to create a connection and check for any errors.

### Exec

```go
import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/go-sql-driver/mysql" // Import your specific database driver
)

func main() {
	// 1. Open a connection
	db, err := sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/test")
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	// 2. Prepare and execute the INSERT statement
	// Use ? for MySQL/SQLite or $1, $2 for PostgreSQL
	query := "INSERT INTO articles (title, content) VALUES (?, ?)"
	result, err := db.Exec(query, "Golang", "Go is here")
	if err != nil {
		log.Fatalf("Insert failed: %v", err)
	}

	// 3. Optional: Get the last inserted ID (not supported by all drivers/DBs)
	id, err := result.LastInsertId()
	if err == nil {
		fmt.Println("New record ID is:", id)
	}
}
```

### QueryRow

`QueryRow` returns one row.

```go
var title string
err = db.QueryRow("SELECT title FROM articles WHERE id = ?", 1).Scan(&title)

if err == sql.ErrNoRows {
	fmt.Println("No record found with that ID")
} else if err != nil {
	log.Fatal(err)
} else {
	fmt.Println("Article title:", title)
}
```

### Query

`Query` return many rows.

```go
rows, err := db.Query("SELECT id, title, content FROM articles WHERE id > ?", 1)
if err != nil {
	log.Fatal(err)
}
defer rows.Close()

type Article struct {
	Id      int
	Title   string
	Content string
}

var articles []Article

for rows.Next() {
	var a Article

	if err := rows.Scan(&a.Id, &a.Title, &a.Content); err != nil {
		log.Fatal(err)
	}

	articles = append(articles, a)
}
fmt.Println(articles)
```
