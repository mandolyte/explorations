+++
title = "Gofpdf: Creating a Pdf From a Text File"
date = 2018-04-19T21:24:22-04:00
draft = false
tags = ["pdf","go"]
categories = []
+++

I plan to create some examples using the gofpdf package,
hopefully useful in their own right.

The first example creates a PDF file from a text file or STDIN.
Here are a couple of use cases taken from the "readme":

- Example 1: create a PDF of the source code, using a courier font.

```bash
go run textToPdf.go -i textToPdf.go -font Courier -o $HOME/data/textToPdf.pdf
```

- Example 2: cram as much as possible onto the page,
using 8 point font and 8 point line height.

```bash
go run textToPdf.go -lh 8 -fs 8 -i textToPdf.go -font Courier -o $HOME/data/textToPdf.pdf
```

The repo is [here](https://github.com/mandolyte/pdf-utils).
I hope to create some other examples soon!
