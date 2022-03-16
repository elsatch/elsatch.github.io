---
title: "Loading private hugo modules from Gitlab and publish to Netlify"
description: "meta description"
image: "images/post/loading-private-hugo-modules-from-gitlab/loading-private-hugo-modules-from-gitlab.png"
date: 2022-02-20T01:10:00+02:00
categories: ["hugo"]
type: "featured" # available types: [featured/regular]
draft: true
mermaid: true
---
#### Introduction

Recently, I have stumbled upon a new need using Hugo. I am creating a new site based on a private repository. Following my previous post, I am setting the theme as a [Hugo module]({{< ref "error-using-hugo-theme-as-a-module.md" >}}). This theme is clone from my public Github repo into a private Gitlab repo.

To publish the site, I will use Netlify again. I will authenticate using my Gitlab credentials and point to the private theme. Every time I commit any changes to my website repo, Netlify will rebuild the site.

So the question arises: How can I make Netlify access the content of the Hugo module, given it is stored in a private repo too?

#### TODO Configuration basics

Two weeks ago, I found an article explaining that [you can register your theme as a Hugo module](https://www.hugofordevelopers.com/articles/master-hugo-modules-managing-themes-as-modules/) instead of as a git submodule. In that way, when Hugo builds the site, it loads all registered Hugo modules automatically and you don't have to deal with submodules. It was pure Hugo bliss...not.

##### TODO Mermaid schema

#### Setting up access to the private Hugo module

#### Conclussion

Photo by Perico of the Palotes
