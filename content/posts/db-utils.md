+++
title = "Database Utilities"
date = 2018-04-20T10:03:59-04:00
draft = false
tags = ["databases","go"]
categories = []
+++

I have completed some utilities for databases.
This post is an overview of what it available.
Other posts will contain details on each...
or you can just read the READMEs if you can't wait.

The repo is [here](https://github.com/mandolyte/db-utils).

There are three utilities:

- *pgcopy*: This utility will use the Postgres
[Copy API](https://godoc.org/github.com/lib/pq#CopyInSchema)
to import data into a Postgres database.
It is one of the fastest ways to import data.
The data to be imported must be a CSV file and
the CSV must have column headers that match the target table.
- *modifydb*: This utility will run a modification SQL statement,
which makes it useful for scripting. It will also take an optional
input CSV file and run an SQL statement for each row
using the columns as substitution parameters.
In the README examples, I use an INSERT statement in combination
with an input CSV to essentially import the CSV into the database.
- *querydb*: This utility will run a query and
output the results to a CSV file. Similar to "modifydb",
it can also take an optional CSV file as input.
In this case, the query is run once per row of input
and the column values in each row is used as substitution parameters.
This technique is useful to create a pipeline of queries,
each using the output of the previous to create a new dataset.
When faced with a big data problem, it is often useful to decompose
a large complex query into smaller ones and chain them together - which
is why this is designed the way it is.

There is also a folder in the repo named "data_prep".
I added it to the repo to show how to create a nice hierarchical dataset.
I found such datasets hard to find.
So I created a process to create a hierarchical dataset
using the Linux filesystem (Ubuntu 17.10).

If you think of the dataset as a graph, then the nodes
are the filenames and folder names (fully qualified).
This data is in the "node" table.
The edge table contains a "from" and a "to"
linking folders to its child folders and its files (leaf nodes). 

Hope you find this useful!
