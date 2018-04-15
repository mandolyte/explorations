+++
title = "Using Chromebook for Go Development"
date = 2018-04-14T21:40:30-04:00
draft = false
tags = []
categories = []
+++

The process for installing a number of Linux variants is fairly
well documented and findable with web searches.
I have spent some time playing with this and have settled
on an approach that works well for me.

I started with "how to geek" article
[here](https://www.howtogeek.com/162120/how-to-install-ubuntu-linux-on-your-chromebook-with-crouton/).
But I found that a full up X Windows desktop is a bit heavy.
As someone else remarked, I just want to do a bit of work
from the couch sometimes and not go downstairs to my office.

Then I came across the article
[here](https://solarianprogrammer.com/2017/09/11/two-weeks-programming-chromebook-challenge/).
I found that there are quite a few things that can be installed
short of an X desktop, in particular, a simple command line interface!
Since I know "vi" and have been wanting to learn vim-go,
I was able to knock off two things at the same time.

So instead of installing a crouton X desktop, I used this:

```
sudo sh ~/Downloads/crouton -t cli-extra
```

The command line is Ubuntu, so the full set of apt-get is at my disposal.
One thing that took some time is discovering that my Samsung Chromebook Plus
has an ARMv8 processor. This is key to installing the correct version of Go.

Blogging from my couch :-),
Cheers!
