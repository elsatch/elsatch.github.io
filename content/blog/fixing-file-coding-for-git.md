---
title: "Fixing file coding for git"
description: "meta description"
image: "images/post/fixing-file-coding-for-git/fixing-file-coding-for-git.png"
date: 2022-03-29T10:37:00+02:00
categories: ["editors"]
tags: ["emacs", "git","data preparation"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
Version controling org-files is one of the best ways to prevent any data loss in the future. Given that these files contain all kind of personal and non-public data, I decided to use Gitea, a git server that [can be installed locally using docker](FIXME-gitea.org).

Once the server was ready, I pushed some initial changes. To my surprise, all the contents of the file were registered as a single line separated by "^M" characters. Changes were registered correctly, but I could not check the changes on the commits, as the line was too long to display them.

#### Coding problem
I started exploring the problem and discovered "^M" is the character used to encode Carriage Returns (CR). Different OS used different conventions on how to mark new lines. Depending on your file encoding, emacs [will do some conversion](https://www.gnu.org/software/emacs/manual/html_node/emacs/Coding-Systems.html) when saving the file. 

The "^M" would mean file was using Classic MacOS encoding (pre-MacOSX 10) which did not make any sense. Was this encoding selected on Doom Emacs configuration? I had no idea, so I asked on the Discord group.

#### Solving the coding problem

Fortunately, someone shared a [link to the relevant section on the emacs manual](https://www.gnu.org/software/emacs/manual/html_node/emacs/Coding-Systems.html).

Checking the docs, I executed `M-x describe-coding-system` and got:
```
Coding system for saving this buffer:
  U -- utf-8-mac (alias: mule-utf-8-mac cp65001-mac)

Default coding system (for new files):
  U -- utf-8-unix (alias: mule-utf-8-unix cp65001-unix)
```

Using `M-x set-buffer-file-coding-system`,` I changed the buffer coding to utf-8-unix and now it is working as expected. Every line appears on a different line when I push in git, so I can check changes easily.

#### Next steps

The only missing piece for me now is: why was the coding system utf-8-mac instead of the default utf-8-unix? I need to verify other files, to see if they are using the right coding too or not.

P.S [This article about end of lines](https://adaptivepatchwork.com/2012/03/01/mind-the-end-of-your-line/) by Tim Clem, a Github worker, explains the challenges on keeping consistency with multiple contributors.

_Photo by Markus Spiske on Unsplash_
