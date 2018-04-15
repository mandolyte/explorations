+++
title = "Encoding Xml Unmarshal"
date = 2018-04-14T21:42:55-04:00
draft = false
tags = []
categories = []
+++

Most examples using Unmarshal show structs that are setup
ahead of time to capture specific contents of the XML document.
While this is the common use case, a struct can be designed to
capture the document in a generic fashion, i.e.,
a struct that works for any XML document.

There are some limitations since this technique relies on
XML struct tags to control the unmarshaling.
The set of tags available do not include
processing instructions or directives.
There are also little quirks:

- The lexical position and number of comments may change.
This is because all comments for an element are collected
into a single one to save into the struct.
- There are no options to control whitespace.
Thus unless the input XML document is a single line,
a lot of entities for whitespace will be saved.
In the sample code I present below, I avoid this by using
`strings.TrimSpace()`.

The key for a generic struct is to use some of the built-in types
and to use slices to reflect one-to-many situations.
Here is the struct:

```go
// note that this struct does not include either
// ProcInst or Directive since tags are not
// provided for them (probably for reasonable cause)
type xmlany struct {
    XMLName xml.Name
    Attrs   []xml.Attr `xml:",any,attr"`
    Content string     `xml:",chardata"`
    Comment string     `xml:",comment"`
    Nested  []*xmlany  `xml:",any"`
}
```

The "Nested" slice will contain pointers to
all child elements of the parent.

As with parseAny.go this example code isn't so useful in its own right,
but mostly serves to illustrate the technique so you can use it for your own use cases.

{{< figure src="/images/xml-generic-unmarshal.png" width="100" title="XML-to-CSV Snippet" class="float">}}

This example is [here](https://github.com/mandolyte/xml-utils).

