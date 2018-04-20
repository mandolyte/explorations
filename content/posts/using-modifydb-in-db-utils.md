+++
title = "Using modifydb in db-utils"
date = 2018-04-20T10:32:35-04:00
draft = false
tags = ["databases", "go", "sql"]
categories = []
+++

The purpose of "modifydb" is to easily execute
an SQL statement that updates a database.
In order to be generic, you may need to update
the source to include the appropriate driver.
As written, it includes two drivers which may
be selected via the "-driver" argument.

Here is a snippet from the source showing the inclusion of two drivers:

```go
import (
 "database/sql"
 ...

 "github.com/mandolyte/db-utils"

 _ "github.com/cznic/sqlite"
 _ "github.com/lib/pq"
)
```

The first one is a native Go driver for SQLite
and is still a work in progress. At this writing,
it works fine for Linux (AMD architectures) and
the Windows Linux Subsystem, but isn't quite ready yet on Windows.

The second one is the standard Postgres driver.



To see the help, use the "-help" argument as shown below.
Notice that the driver name is required, such as "postgres" or "sqlite".
Also required is the name of an environment variable
containing the connect string.

The connect string format varies from driver to driver. Examples are shown later.

```go
$ modifydb -help
Help:
Usage:
  -debug
        Show debug messages (default true)
  -driver string
        Driver name; required
  -help
        Show help message
  -input string
        Optional input CSV supplying parameters to SQL query
  -parameters string
        Comma delimited list of column numbers of input CSV in order needed;
first is 1, not zero
  -query string
        SQL statement filename
  -urlref string
        Environment variable with DB URL

Notes:
1. If the optional input CSV is supplied, then the rows will be used to supply
parameter values to the SQL statement. 
2. The input CSV must be accompanied by a list of column numbers in the order
needed to correctly drive the SQL parameter substitution. 
        For example, if the WHERE clause is:
                WHERE x = ? and (y = ? or z = ?)
        and the values in the CSV for x, y, and z are found in columns 
        2, 12, and 3, then the parameters argument will look like this:
                -parameters 2,12,3
3. The SQL statement will be executed one time for each row in the CSV.
4. The list of parameters is one-based like SQL, not zero based. 
So first column is one, not zero
```

Optionally, a CSV input file may be provided to be used for
substitution parameters.
Which ones to use are specified by the "-parameters" argument.
The notes in the help message have a bit more explanation.

Let's say you want to create a table in the database, using:

```sql
CREATE TABLE edge (
    id text key,
    from_id text not null,
    to_id text not null
)
```

Then a script to do this for SQLite might look like this:

```sh
DB=here.db
DN=sqlite
export DB DN
modifydb -query edge_create.sql -urlref DB -driver $DN
```

In the next example, suppose you have a CSV file
that you want to insert into the table above.
The SQL would look this:

```sql
insert into edge (id,from_id, to_id)
values (?, ?, ?)
```

Let's say that the input CSV columns to use are the first three,
in the same order needed by the insert statement.
Then the command might look like this:

```sh
modifydb -query edge_insert.sql \
    -input $HOME/data/hier/hier_edges.csv \
    -parameters 1,2,3 \
    -urlref DB -driver $DN
```

where the SQL filename is `edge_insert.sql`;
the CSV filename is specified on the `-input` argument;
and the columns to use as substitution parameters
are supplied on the `-parameters` argument.

Hope you find this useful!

