+++
title = "Markdown to PDF"
date = 2018-04-19T21:58:01-04:00
draft = false
tags = ["markdown","pdf","go"]
categories = []
+++

I recently completed the initial release of a Markdown to PDF package.

Repo is named "mdtopdf" and is [here](https://github.com/mandolyte/mdtopdf).
Docs are [here](https://godoc.org/github.com/mandolyte/mdtopdf).

The supported Markdown elements are:

- Emphasized and strong text
- Headings 1-6
- Ordered and unordered lists
- Nested lists
- Images
- Tables
- Links
- Code blocks and back-ticked text

In the "cmd" folder is an example of using the package
and it contains both a test markdown file and the resultant PDF file.
There is a slightly different version of the command line utility
in my "pdf-utils" repo.

The text styles can be modified and the example shows some of this
 using the package's "Styler" struct:

```go
type Color struct {
    Red, Green, Blue int
}

type Styler struct {
    Font      string
    Style     string
    Size      float64
    Spacing   float64
    TextColor Color
    FillColor Color
}
```

See details on limitations and things to do on the README.
