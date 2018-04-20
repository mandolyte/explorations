+++
title = "Gofpdf: Creating a Numbered List"
date = 2018-04-19T21:35:16-04:00
draft = false
tags = ["pdf","go"]
categories = []
+++

This example shows how to create a number list of items
[here](https://github.com/mandolyte/examples-gofpdf/tree/master/textToList).
It includes use of several methods:

- It uses GetStringWidth() to get the point width of the letter "m".
This width is then used to control indentation.
- It uses CellFormat() to right justify the item number at the beginning of the line.
- It uses MultiCell() to conveniently wrap the text and align it to the right of the item number.

The input is a text file where each line is to be used as a numbered item. Any blank lines are skipped.

The example has several options including one to create the list in a compressed form (no blank lines between the items) or not.

Here is the help message:

```bash
$ go run textToList.go -help
Help Message

Usage: textToList [options]
Note: font and line height are 'points', 1/72 inch
This example takes a text file of paragraphs and
demonstrates how to create a numbered list in PDF format
Blank lines are skipped!
  -compressed
        Don't add blank line between items
  -font string
        Font 'Arial', 'Courier', or 'Times'; default Arial (default "Arial")
  -fs float
        Font size; default 12 (default 12)
  -help
        Show usage message
  -i string
        Input text filename; default is os.Stdin
  -lh float
        Line height; default 15 (default 15)
  -o string
        Output PDF filename; required
  -ps string
        Paper size; default 'Letter' (default "Letter")
  -style string
        Style: B for bold, U for underline, I for Italics or any combo thereof
$
```

Hope you find this helpful!
