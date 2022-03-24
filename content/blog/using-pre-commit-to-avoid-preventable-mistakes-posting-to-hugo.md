---
title: "Using pre-commit to avoid preventable mistakes"
description: "meta description"
image: "images/post/using-pre-commit-to-avoid-preventable-mistakes-posting-to-hugo/using-pre-commit-to-avoid-preventable-mistakes-posting-to-hugo.png"
date: 2022-03-25T18:30:00+02:00
categories: ["editors", "workflow"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
A couple of years ago I attended Airflow conference (virtually). One of the workshops was on how to contribute to the project. As I joined that session, I discovered a rich set of tools to produce quality code and prevent mistakes.

How could I translate this to my daily practice?

#### Poka-yoke or the art of reducing errors

Poka-yoke is a Japanese term that describes a series of techniques to prevent mistakes in the workspace. It was introduced by Shigeo Shingo at Toyota as a method to improve the quality of the cars produced. Basically, all machines and elements where mistakes could be introduced were customized in a way that mistakes would be glaring obvious to the operator. By adding these small changes, the overall results were outstanding compared to their competitors.

#### Pre-commit or the art of automated testing

Most of the poka-yokes are physical but there are also software tools that share the same philosophy. Pre-commit is an utility that can run several tests before you commit your changes to a repository.

This blog is powered by Hugo, a static web generator, that rebuilds the site when I commit new changes to the website repository. When I want to publish something new, I just need to commit and push my updates. Github actions will incorporate all new changes.

Unfortunately, mistakes can happen when publishing: images may be missing, markdown can have some typos, etc. My plan is to check for the bare basics using [Pre-commit](https://pre-commit.com/). There are hundreds of pre-built packages but: what do I need right now?

##### My dream pre-commit configuration

These are the checks I would love to have automated for every commit!

- Checks for Markdown comment syntax:
  - There is a new line below the comment
  - Every comment finishes with a double comma symbol (otherwise they are rendered as regular text)
  
- Checks for every picture:
  - Every post has a markdown file
  - There is a folder for the main photo, named as the post filename, under the assets/images route
  - There is a folder for the additional photos, named as the post filename, under the static/images route
  - All images referenced in the post exist as files in the correct locations.

- Verifies Markdown is compliant and optionally fixes any mistakes
- Verifies frontmatter is toml compliant
- Verifies mermaid is set to false in the frontmatter, unless there is a mermaid call in the body of the post.

- Checks publication date and verifies if it is a duplicate of any previous posts.
  
- Checks all links in the new article, to verify the websites are reachable.

##### Work ahead!
In a future post, I will try to configure all of these rules using the available modules at Pre-Commit website. Stay tuned!

_Photo by Agence Olloweb on Unsplash_
