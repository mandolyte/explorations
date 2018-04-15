+++
title = "Xml Identity Transform"
date = 2018-04-14T21:52:09-04:00
draft = false
tags = []
categories = []
+++

An identity transformation is one that reproduces the input.
The article [here](https://en.wikipedia.org/wiki/Identity_transform),
shows examples using XML Stylesheets and XQuery.

The Go XML package cannot exactly do an identity transform
because the standard marshal function does not handle
processing instructions or directives.
And because the marshal function relies on a struct to save the data,
it cannot keep track of the relative position of comments either.
However, it will associate the comment to the correct parent element,
but may move it compared to the input XML document.

In theory, an encoding of decoded XML document would be able to produce
an exact replica. However, the code below results in an error:

```go
package main

import (
 "encoding/xml"
 "fmt"
 "os"
 "strings"
)

func main() {
 data := `
 <Person>
  <FullName>Grace R. Emlin</FullName>
  <Company>Example Inc.</Company>
 </Person>
`
 decoder := xml.NewDecoder(strings.NewReader(data))
 encoder := xml.NewEncoder(os.Stdout)
 if err := encoder.Encode(decoder); err != nil {
  fmt.Printf("error: %v\n", err)
 }
}
$ go run identityxform.go
error: xml: unsupported type: map[string]string
$ 
```

Since that doesn't work, I used the marshal and unmarshal functions
with the generic tagged struct using in the previous blog article.
And by using the indent variant of the marshal function,
the code will pretty print.

Here is an example:

```bash
$ cat test1.xml 
<Person type="alien" thumbs="yes">here is some character data<FullName>Grace R. Emlin</FullName><!-- this is not a comment... just kidding --><Company>Example Inc.<!-- this company excels --></Company></Person>
$ go run identityXform.go -i test1.xml -indent
<Person type="alien" thumbs="yes">here is some character data
    <!-- this is not a comment... just kidding -->
    <FullName>Grace R. Emlin</FullName>
    <Company>Example Inc.
        <!-- this company excels --></Company>
</Person>
$
```

Find the code and readme under "identityXform" [here](https://github.com/mandolyte/xml-utils).

Hope you find this series helpful!
