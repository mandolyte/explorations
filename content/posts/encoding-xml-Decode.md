+++
title = "Encoding Xml Decode"
date = 2018-04-14T21:41:38-04:00
draft = false
tags = ["xml"]
categories = []
+++

# Background
I plan to explore a number of Go packages. These will often
come with useful samples or utilities. Enjoy!

All the code for this post is in this [repo](https://github.com/mandolyte/xml-utils).

# A Generic XML Parser
![Parse XML output](/images/xml-parse-any-output.png#float-right)
The first example will be a generic parser: `parseAny.go`.
This example will parse any XML, identify all element and attributes
and output a CSV file as shown. The input XML for this is below.


The depth of an element and the element type are two necessary
ingredients to create a pretty printer Go program.

While this code may be useful to get a quick understanding of
content of an XML file, its likely to be more valuable as a
starting point to tweak and make into something you need.

Here is the input XML for the above:

```xml
<?target instructions?>
<!this-is-a-directive>
<Person>
<FullName>Grace R. Emlin</FullName>
<Company>Example Inc.</Company>
<Email where="home">
<Addr>gre@example.com</Addr>
</Email>
<Email where='work'>
<Addr>gre@work.com</Addr>
</Email>
<Group>
<Value>Friends</Value>
<Value>Squash</Value>
</Group>
<City>Hanga Roa</City>
<State>Easter Island</State>
<!-- this is a comment -->
</Person>
```
