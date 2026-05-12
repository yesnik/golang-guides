# Database

## Sql

Package `sql` provides a generic interface around SQL (or SQL-like) databases. The `sql` package must be used in conjunction with a database driver.

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

