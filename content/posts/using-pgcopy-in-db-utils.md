+++
title = "Using pgcopy in db-utils"
date = 2018-04-20T10:17:04-04:00
draft = false
tags = ["databases","go","postgres"]
categories = []
+++

The "pgcopy" utility will import a CSV file using the
"lib/pq" copy-in-schema function.
It is one of the fastest ways to import into a Postgres database.

Using the "-help" options shows:

```bash
$ pgcopy -help
Help:
  -help
        Show help message
  -input string
        Input CSV to COPY from
  -schema string
        Schema with table to COPY to
  -table string
        Table name to COPY to
  -urlvar string
        Environment variable with DB URL
The input CSV file must have headers that match the table names
$
```

The schema and table name are given by separate arguments.
The connection string uses the "lib/pq" format like this: 
`postgres://pqgotest:password@localhost/pqgotest?sslmode=verify-full`.

This string must be stored in an environment variable.
It is the variable name that is specified to the "urlvar" argument.

Here is an actual command, preceded by exporting the DB URL:

```sh
DBURL="postgresql://postgres:postgres@localhost/postgres?sslmode=disable"
export DBURL

pgcopy \
    -schema public \
    -table node \
    -input $HOME/data/hier/hier_nodes.csv \
    -urlvar DBURL
```
 
Finally, notice that the CSV file must have headers
that match the column names in the table (case insensitive).
