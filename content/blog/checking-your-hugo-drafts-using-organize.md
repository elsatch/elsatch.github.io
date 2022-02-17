---
title: "Checking your Hugo drafts using organize-tool"
description: "meta description"
image: "images/post/getting-help-in-python.png"
date: 2022-02-09T14:00:00+02:00
categories: ["python", "organize-tool"]
type: "regular" # available types: [featured/regular]
draft: true
mermaid: true
---
#### Introduction

Last weeks, I have been working on organizing my files using Organize tool. At the same time, I also started this blog. Most days I sketch some thoughts, notes and ideas that end up in a draft located in my blog folder.

To create this blog I am using a static page generator called Hugo. As I upload the changes to github, this website updates seamlessly using github actions. All posts in this site start as a simple markdown file in my hard drive.

How can I check which files are drafts and which ones are already published? Organize-tool to the rescue.

#### Configuring organize-tool

Organize works with a default config file that holds all the rules for your files and folders. As I want to run this action from command line, I will use an alternative route, passing the configuration file as a parameter.

In organize v1, the information on how to pass an configuration file was available with `organize --help` command but it is gone from the v2 version. Fortunately, this information is available at the [updated documentation](https://organize.readthedocs.io/en/v2.1.2/configuration/#running-and-simulating).

To pass a config file as a parameter:

```console
organize sim [FILE]
organize run [FILE]
```

Note that you can also pass a working dir as an argument, but we won't use that feature for this particular example.

To create the config file we have to define three parameters:

- Location: the folder containing the drafts
- Filters: that will check for markdown files containing the string "draft: true"
- Action: that will echo a message to the console with the draft files

This was my initial config file (that failed):
```yaml
rules:
  - name: Check Hugo drafts
    locations: "C:\Users\Mi PC\dev\elsatch.github.io\blog"
    filters:
      - extension: md
      - filecontent: 'draft: true'
    actions:
      - echo: "Draft post: {filename}"
```

Organize includes a parameter to check a config file. As I run `organize check check_drafts.yml` I got an error:

```console
ValueError: Invalid location C:\Users\Mi PC\dev\elsatch.github.io\blog (root path 'C:\Users\Mi PC\dev\elsatch.github.io\blog'
does not exist)
```

There were two causes for the error:

- The folder of the drafts is not at blog but under content\blog
- Escape characters on the string prevented the program to parse the route properly.

If you plan to use Organize in Windows, you need to pass double backslashes to make it work! Once I fixed those mistakes, I launched `organize check` again and... it worked!

```console
organize.exe check .\check_drafts.yml
Checking: check_drafts.yml
Config is valid.
```

This is the file that passed the check:

```yaml
rules:
  - name: Check Hugo drafts
    locations: "C:\\Users\\Mi PC\\dev\\elsatch.github.io\\content\\blog"
    filters:
      - extension: md
      - filecontent: "draft: true"
    actions:
      - echo: "Draft post: {filename}"
```

#### Testing the configuration


Getting your file to pass the `organize check` command means that the syntax and routes on your config file do not contain errors. But we have to check now if our program works in the way we expect it, by listing the files that have the string: "drafts:true".

```console
organize sim .\check_drafts.yml
organize 2.1.2
Config: "check_drafts.yml"

┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ SIMULATION                                                                                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

⚙ Check Hugo drafts ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
/C:\Users\Mi PC\dev\elsatch.github.io\content\blog\
  checking-your-hugo-drafts-using-organize.md
    - (filecontent) ERROR! incomplete escape \u at position 2
  getting-interactive-help-in-python.md
    - (filecontent) ERROR! incomplete escape \u at position 2
  introduction-to-organize-tool.md
    - (filecontent) ERROR! incomplete escape \u at position 2
  launching-a-blog-in-2022.md
    - (filecontent) ERROR! incomplete escape \u at position 2
  my-organize-use-cases.md
    - (filecontent) ERROR! incomplete escape \u at position 2
  _index.md
    - (filecontent) ERROR! incomplete escape \u at position 2

┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ SIMULATION                                                                                                                                                              │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

Nothing to do.
```

So we got an error on the filecontent filter. Checking the documentation, this filter requires us to pass a regular expression instead of a simple string. We need to rewrite our filter to make it work.

It's been a long time since I had to do RegEx to match, so I opted for using [RegEx101 website](https://regex101.com/).

As I typed the string, I saw the regexp did not match my string so I launched the debugger option.

# TODO Meter aquí captura de imagen

After typing the string again, selecting the Python regexp I saw it should work with my given string. Hummm, maybe the errror is then related to the string parsing in Organize?

I checked the issues in the repository and found an intriging [issue regarding matching multiple words](https://github.com/tfeldmann/organize/issues/142)

Issue is still open and without response, but gave me an idea to test with the following config file:

```yaml
rules:
  - name: Check Hugo drafts
    locations: "C:\\Users\\Mi PC\\dev\\elsatch.github.io\\content\\blog"
    filters:
      - extension: md
      - filecontent: '(?P<all>.*)'
    actions:
      - echo: '{filecontent.all}'
```

But still no luck at all! I will try to troubleshoot this problem calling the regexp directly from Python.

#### Checking if a string exists in a file using Python regexps


So it will look like this:
{{<mermaid>}}
graph LR;
    id1([Loc: Drafts folder])
    id2([Filter: Extension])
    id3([Filter: FileContent])
    id4([Action: Echo])
    id1 --> id2;
    id2 --> id3;
    id3 --> id4;
{{</mermaid>}}

Warning: As in previous posts, I will be using Organize-tool v2. The syntax in this example will not work with previous versions.





```sh
ValueError: Invalid location C:\Users\Mi PC\dev\elsatch.github.io\blog (root path 'C:\Users\Mi PC\dev\elsatch.github.io\blog'
does not exist)
PS C:\Users\Mi PC\dev\ejecutables> organize.exe check .\check_drafts.yml
Checking: check_drafts.yml
Config is valid.
```

#### Other alternatives to achieve the same results
