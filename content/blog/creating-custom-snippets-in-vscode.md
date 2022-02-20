---
title: "Create custom snippets in VSCode"
description: "meta description"
image: "images/post/creating-custom-snippets-in-vscode/create-custom-snippets-in-vscode.png"
date: 2022-02-18T16:00:00+02:00
categories: ["vscode", "editors", "workflow"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
#### Introduction

If you need to write several times the same shortcodes or writable incantations, it might make sense to use snippets.

Snippets are small fragments of code what can be inserted easily in your editor, using some keyword combination. In VSCode you can insert a snippet by typing the first characters and pressing `Ctrl+Space`.

#### Keeping track of pending TODO's

I am so used to using Org-mode in emacs that I miss the chance to add TODO's in any part of my documents. When I am writing new posts, I would love to to * TODO task, with the super simple org syntax. However, we are in markdown land and adding comments or tasks in the plain text is not that simple.

After browsing Stack Overflow, it seems like the most cannonical way to add comments in Markdown, that won't be rendered into the final document is:

```markdown
[//]: # "Comment"

```

Warning! Note that there is a blank line after the comment!

There is an extension for VSCode that looks for the keywords TODO and FIXME in the current workdir and offers a plain list. So I decided to create a snippet to insert the TODO's in a simple way in the markdown file that Hugo uses. It is super simple to create a snippet in VSCode (at least coming from Emacs YasSnippet land)

#### How to create a custom snippet in VSCode

To create the custom snippet in VSCode, you just need to click `File-> Preferences-> User Snippets`. A menu will pop-up asking if you want to create a new snippet file or add your snippet to one of the existing languages already defined. I choose markdown file an added the following lines:

```json
"Add Markdown TODO Comment": {
            "prefix": "add",
            "body": "[//]: # \"TODO: $0 \" \n",
            "description": "Add Markdown TODO Comment"
            }
```

Breaking down the body, I am asking VSCode to insert the [//] symbol, followed by the hashtag, then a escaped double comma before the keyword TODO (followed by double dot) that marks the beginning of the comment. Then I mark where I would like my cursor to go after pasting the snippet by using $0. There is a closing double comma followed by the new line character.

To insert the snippet I just need to type add and press `Ctrl+Space` and I will get my new TODO inserted.

To check all my TODOs I just need to open the command palette by pressing `Ctrl+Shift+P` and then choose the command list highlighted annotations.

![command palette](images/post/creating-custom-snippets-in-vscode/list-highlight-todo.png).

All my pending TODOs will be listed under the OUTPUT tab.

![sample list of pending todos](images/post/creating-custom-snippets-in-vscode/list-of-pending-todos.png)

And now, it is time to work on those TODO's!!

Photo by Sonja Prein on Unsplash
