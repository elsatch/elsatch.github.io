---
title: "Creating fast presentations using orgmode and reveal"
description: "meta description"
image: "images/post/creating-fast-presentations-using-orgmode-and-reveal/creating-fast-presentations-using-orgmode-and-reveal.jpg"
date: 2022-07-29T01:13:30+02:00
categories: ["emacs", "orgmode"]
type: "featured" # available types: [featured/regular]
draft: true
mermaid: false
---
#### Introduction

In this brief post I'll explain how to create fast presentations using emacs + orgmode + reveal.

#### Requisites

- emacs
- orgmode
- ox-reveal

#### Installing OX-Reveal

OX-Reveal module creates a new entry in the export menu to convert your Org file to Reveal. To install it under Doom Emacs, open your `config.el` and add the following lines:

```lisp
(after! org
     (load-library "ox-reveal")
     (setq org-reveal-root "file:///path/to/reveal.js-master"))
```

Note: Don't worry about the org-reveal-root, we will set reveal options in the files themselves.

Once it is installed, you can just do `C-c C-e R B` to create your presentation.

#### Creating a new presentation

To create a new presentation, create a new orgmode file. Paste the following code at the top of the file:

```
:PROPERTIES:
:DIR:      ~/org/projects/notebook-a-modelo-presentacion/img/
:END:

:REVEAL_PROPERTIES:
#+REVEAL_ROOT: https://cdn.jsdelivr.net/npm/reveal.js
#+REVEAL_REVEAL_JS_VERSION: 4
#+REVEAL_THEME: night
#+OPTIONS: timestamp:nil toc:nil num:nil tags:nil todo:nil
:END:

#+TITLE: De Notebook a modelo desplegado como servicio web
```

The `DIR` property is the route where images will be saved. This needs to be setup per presentation using the command `C-c a s` for `(org-attacg-set-directory)`. Additional info: https://orgmode.org/manual/Attachment-defaults-and-dispatcher.html

To insert an image fast, copy it on your clipboard and then run `M-x org-download-clipboard`. Usually I create a screenshot of an area of the screen using `cmd-shift-4`, then click the thumbnail that appears, add some notes, arrows, etc. Then I open it using Preview, then copy it to the clipboard. A faster way to do it if you don't want to customize the screenshot is running `M-x org-download-screenshot`, or running the shortcut `C-M-y`.

Now, you can start creating headings as in any regular orgmode file:

- Top level headings are new slides
- Secondary level headers create horizontal splits in your presentation
- To split a slide in two, because contents are too long, you can just insert `#+REVEAL: split:t` at the point you want to split
- To add code, insert a orgbabel section using the shortcut `<s`then press TAB.

#### And that's it!

With these simple elements you can create a nice looking presentation, created from pure text, that can be versioned, opens in any browser, etc.

_Photo credits: Photo by Teemu Paananen on Unsplash._
