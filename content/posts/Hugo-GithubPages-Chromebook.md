+++
title = "Hugo GithubPages Chromebook"
date = 2018-04-14T21:40:24-04:00
draft = false
tags = []
categories = []
+++

I confess that there is little to like about Google's Blogger platform.
It certainly has not kept pace with offerings like Wordpress or Medium.
I suspect that it would suffice for normal text only blogging, but 
I have a technical blog and getting code snippets to work well is painful.

So when I learned recently that Github Pages supported Jekyll I
immediately did a search to see if Hugo supported Github Pages.
To my delight it did. Furthermore, since blogging is often a
"sofa" activity, I decided to try to use my Chromebook from the start.
I have a Samsung Chromebook Plus, which uses an ARM chip and often
means a little more work. I have crouton installed using the target
"cli-extra", which means no GUI... just Vim and me (1).

First I tried to just use apt-get to install Hugo, but that version
is over two years old and couldn't even do the quickstart tutorial (2).
So I used the tarball instructions to install
`hugo_0.38.2_Linux-ARM64.tar.gz`.
I installed as directed into `$HOME/bin` and ensured that it was 
on my path. Transcript:

```bash
$ tar -xvzf ~/Downloads/hugo_0.38.2_Linux-ARM64.tar.gz 
README.md
LICENSE.md
hugo
$ ./hugo version
Hugo Static Site Generator v0.38.2 linux/arm64 BuildDate: 2018-04-09T08:17:45Z
$ which hugo
/home/cecil/bin/hugo
```

I found out quickly that when I started the hugo web server
to view my content, I was able to simply switch tabs and go to
`localhost:1313` to view... and it worked perfectly.
If unable view the content, then using my Chromebook
was pretty much a non-starter.

There are several ways to setup Hugo to work with Github Pages (3).
Since I wanted to be able to have multiple blogs, I settled on
the "project" based approach.

By far the hardest part of using Hugo is picking a theme (4).
There is a wide variety of themes, which is a good thing.
But the themes have little consistency and support a bewildering
set of parameters and features, which makes it hard.
I soon learned to look at the list of configuration parameters 
in the theme documentation to evaluate whether a theme had what I wanted.

I finally settled on the Kiera theme
(as a fan of Vlad, how could I resist??).
It is simple, clean, and rendered my test post very well,
which included code blocks, backticked phrases, bullets, images, links,  etc.
However, it did not support Google Plus as a social network option,
which I wanted.
So I had to research a bit and ended up forking the theme
and doing a pull request to add it.

In the hours I played with different themes,
I finally got into a rhythm on evaluating them:

- Create a new directory for the theme
(if tried to keep multiple themes in one project, the configuration
parameters seemed to interfere, so I kept them cleanly separated)
- Copy over content, static folders
- Use the theme's `config.toml` file as a starting point and update it to suit
- Finally start the server and test it: `hugo server -D`

Since these are static web pages, there is no built-in commenting system.
Disqus is available, but has mixed reviews. So I simply suggest that
if you want to comment, contact me over on LinkedIn or Google+ (see top
of each post)... or file an issue in Github! :-)

My first set of posts will be ones ported over from Blogger.

*References*

1. http://www.mandolyte.info/2018/02/using-chromebook-for-go-development.html
2. https://gohugo.io/getting-started/quick-start/
3. https://gohugo.io/hosting-and-deployment/hosting-on-github/
4. https://themes.gohugo.io/
