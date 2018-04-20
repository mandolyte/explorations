+++
title = "Using querydb in db-utils"
date = 2018-04-20T10:44:45-04:00
draft = false
tags = ["databases","sql","go", "csv"]
categories = []
+++

The purpose of "querydb" is to easily execute an
SQL SELECT statement at the command line or in a script.
As with "modifydb", in order to be generic,
you may need to update the source to include the appropriate driver.
As written, it includes two drivers which may be selected
via the `-driver` argument.

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
and is still a work in progress.
At this writing, it works fine for Linux (AMD architectures)
and the Windows Linux Subsystem, but isn't quite ready yet on Windows.
The second one is the standard Postgres driver.

To see the help, use the "-help" argument as shown below.
Notice that the driver name is required,
such as "postgres" or "sqlite".
Also required is the name of an environment variable
containing the connect string.
The connect string format varies from driver to driver.
Examples are shown later.

```sh
$ querydb -help
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
  -output string
        Output CSV filename
  -parameters string
        Comma delimited list of olumn numbers of input CSV in order needed;
first is 1, not zero
  -query string
        SQL statement filename
  -urlref string
        Environment variable with DB URL

Notes:
1. If the optional input CSV is supplied, then the rows will be used to supplay
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

Optionally, a CSV input file may be provided that will be
used to for substitution parameters.
Which ones to use are specified by the `-parameters` argument.
The notes in the help message above have a bit more explanation.

Suppose you wanted to run this query and it was in filename "q1.sql":

```sql
select count(*) from node
union all
select count(*) from edge
```

To store the query result on "q1.csv" the commands might look like this:

```sh
DN=postgres
DB="postgresql://postgres:postgres@localhost:5432/postgres?sslmode=disable"
export DN DB

querydb \
    -query q1.sql \
    -output q1.csv \
    -driver $DN \
    -urlref DB
```

There is an optional input CSV as well.
When specified it will run the SQL for each row in the CSV.
The CSV must have headers on the first row,
which is skipped of course.
The output CSV will be columns from the input CSV
plus the results of the query as added columns.
If the result has multiple rows
then the output CSV will repeat the input values as needed.

Here is an example. Given this little CSV:

```csv
val1,val2
b,p
c,e
```

and this SQL, say on "q2.sql":

```sql
select id
from node
where id like ($1 || '%')
and id like ('%' || $2)
```

Then the SQL twice will be executed twice.

- The first time with substitution parameters "b" and "p" for "$1" and "$2".
- The second time with "c" and "e".

If the input CSV was on "input2.csv", then the command to execute would look like this:

```sh
DN=postgres
DB="postgresql://postgres:postgres@localhost:5432/postgres?sslmode=disable"
export DN DB

querydb \
    -input input2.csv \
    -parameters 1,2 \
    -query q2.sql \
    -output q2.csv \
    -driver $DN \
    -urlref DB
```

And the output will be like this (just showing head and tail of output file):

```sh
$ head q2.csv
val1,val2,id
b,p,bzcmp
b,p,bzegrep
b,p,bzfgrep
b,p,bzgrep
b,p,brltty-setup
b,p,bitmap
b,p,bcp
b,p,benchcmp
b,p,bluetooth-sendto.desktop

$ tail q2.csv
c,e,circle
c,e,crossed_circle
c,e,cross_reverse
c,e,compcache
c,e,changelog-file
c,e,copyright-file
c,e,corrections-case
c,e,clocksource
c,e,cleanfile
c,e,coccinelle
$
```

Notice that the input is repeated for as many rows as needed.
The output now has three columns;
the first two from the input CSV and
the last column from the executed SQL result.

Hope you find this useful!
