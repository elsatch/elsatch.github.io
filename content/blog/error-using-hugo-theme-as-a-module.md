---
title: "Error building my web when using Hugo Theme as a Hugo Module"
description: "meta description"
image: "images/post/hugo-theme-as-hugo-module.png"
date: 2022-02-09T14:00:00+02:00
categories: ["hugo"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
#### Introduction

I have choosen Geeky Hugo as the theme for this site. I forked the original repo to my Github account. Then I wanted to add support for Mermaid Diagramms. So I added Mermaid as a Hugo module and added a shortcode file for Mermaid in the theme directory. Then hell broke lose a few commits afterwards!

#### What do you mean loading a Hugo theme as a Hugo module

If you check Hugo documentation, you will discover that most themes include some lines about adding the Hugo theme as a submodule. That is: you will clone the repo under the themes folder and register it as a git submodule. In this way, you will be able to update your theme easily fetching the changes upstream.

I tried this setup, using vscode as editor. I started editing in a couple of machines and started getting errors about theme version mismatch when loading submodules. I tried getting updates, syncing the changes across but that failed too.

Two weeks ago, I found an article explaining that [you can register your theme as a Hugo module](https://www.hugofordevelopers.com/articles/master-hugo-modules-managing-themes-as-modules/) instead of as a git submodule. In that way, when Hugo builds the site, it loads all registered Hugo modules automatically and you don't have to deal with submodules. It was pure Hugo bliss...not.

#### Error building the site

I registered my theme Geeky-Hugo as Hugo module. Then I changed my config.toml to comment the theme line and add the module line. Everything should work perfectly.

As I tried to build my site, I got this error both in my local computer and on the Github Actions build log:

```console
Run hugo --minify
4
hugo: downloading modules …
5
hugo: collected modules in 25224 ms
6
Start building sites … 
7
hugo v0.91.2-1798BD3F+extended linux/amd64 BuildDate=2021-12-23T15:33:34Z VendorInfo=gohugoio
8
ERROR 2022/02/19 22:15:26 render of "page" failed: execute of template failed: template: _default/contact.html:18:4: executing "_default/contact.html" at <partialCached "style.html" .>: error calling partialCached: "/tmp/hugo_cache/modules/filecache/modules/pkg/mod/github.com/elsatch/geeky-hugo@v0.0.0-20220219095841-700f69e277aa/layouts/partials/style.html:30:33": execute of template failed: template: partials/style.html:30:33: executing "partials/style.html" at <resources.Concat>: error calling Concat: resources in Concat must be of the same Media Type, got "text/x-scss" and "text/css"
9
ERROR 2022/02/19 22:15:26 render of "page" failed: execute of template failed: template: _default/single.html:18:4: executing "_default/single.html" at <partialCached "style.html" .>: error calling partialCached: "/tmp/hugo_cache/modules/filecache/modules/pkg/mod/github.com/elsatch/geeky-hugo@v0.0.0-20220219095841-700f69e277aa/layouts/partials/style.html:30:33": execute of template failed: template: partials/style.html:30:33: executing "partials/style.html" at <resources.Concat>: error calling Concat: resources in Concat must be of the same Media Type, got "text/x-scss" and "text/css"
10
ERROR 2022/02/19 22:15:26 render of "page" failed: execute of template failed: template: _default/search.html:18:4: executing "_default/search.html" at <partialCached "style.html" .>: error calling partialCached: "/tmp/hugo_cache/modules/filecache/modules/pkg/mod/github.com/elsatch/geeky-hugo@v0.0.0-20220219095841-700f69e277aa/layouts/partials/style.html:30:33": execute of template failed: template: partials/style.html:30:33: executing "partials/style.html" at <resources.Concat>: error calling Concat: resources in Concat must be of the same Media Type, got "text/x-scss" and "text/css"
11
ERROR 2022/02/19 22:15:26 render of "page" failed: execute of template failed: template: _default/single.html:18:4: executing "_default/single.html" at <partialCached "style.html" .>: error calling partialCached: "/tmp/hugo_cache/modules/filecache/modules/pkg/mod/github.com/elsatch/geeky-hugo@v0.0.0-20220219095841-700f69e277aa/layouts/partials/style.html:30:33": execute of template failed: template: partials/style.html:30:33: executing "partials/style.html" at <resources.Concat>: error calling Concat: resources in Concat must be of the same Media Type, got "text/x-scss" and "text/css"
12
ERROR 2022/02/19 22:15:26 failed to render pages: render of "page" failed: execute of template failed: template: _default/about.html:18:4: executing "_default/about.html" at <partialCached "style.html" .>: error calling partialCached: "/tmp/hugo_cache/modules/filecache/modules/pkg/mod/github.com/elsatch/geeky-hugo@v0.0.0-20220219095841-700f69e277aa/layouts/partials/style.html:30:33": execute of template failed: template: partials/style.html:30:33: executing "partials/style.html" at <resources.Concat>: error calling Concat: resources in Concat must be of the same Media Type, got "text/x-scss" and "text/css"
13
Error: Error building site: TOCSS: failed to transform "style.scss" (text/x-scss): SCSS processing failed: file "/tmp/hugo_cache/modules/filecache/modules/pkg/mod/github.com/elsatch/geeky-hugo@v0.0.0-20220219095841-700f69e277aa/assets/scss/bootstrap/_mixins.scss", line 6, col 1: File to import not found or unreadable: vendor/rfs. 
14
Total in 25290 ms
15
Error: Process completed with exit code 255.
```

As I searched for this error, I stumbled upon multiple suggestions about using Hugo Extended version to get the SCSS files processed. I am using Hugo 0.92.1 Extended version, so that should not be an issue.

Then I focused on the last line, as it was the one that differed from other error logs I have seen online. It says: `File to import not found or unreadable: vendor/rfs.`.

Fortunately, I found an issue related to this problem. @bep, one of Hugo project leaders [explained in that thread](https://github.com/gohugoio/hugo/issues/6945#issuecomment-590270825):

```quote
OK, so the problem is a bug in Go. I have reported it upstream and I assume it will get priority, but one never knows.

In Go Modules, the top-level /vendor directory in a module has a special meaning, so they delete it from the bundle after downloading. Sadly, their regexp (or whatever) is a little aggressive, so this folder goes as well:

https://github.com/twbs/bootstrap/tree/master/scss/vendor

Note that if you mount that folder from the project itself, you will not experience this.

There may be other workarounds until this gets fixed upstream, but you would do me a solid if you could rename this folder to something else (vendors, _vendor... whatever).
```

Ok, so that is the root cause. From what I understand, as the Go program is building your Hugo website, it cleans up afterwards any folders named vendor. Some of the files used by Bootstrap are saved in a folder called vendor, that gets deleted afterwards. When the main program tries to process the scss files, there are some components missing and it fails. This only happens with Go/Hugo Modules, so if you store the files locally, the error will disappear, but we will go back to the previous scenario.

#### Possible fix

In the thread, they point to a potential fix. It is a Hugo module called [hugo-mod-bootstrap-css](https://github.com/gohugoio/hugo-mod-bootstrap-scss/tree/main) that bundles Bootstrap V4 and V5 components, that should fix the problem. To use it, you have to add in your config file the following lines:

```toml
[module]
[[module.imports]]
path = "github.com/gohugoio/hugo-mod-bootstrap-scss/v5"
```

#### Realtime test

If you can see this line, this fix solves the problem of Geeky Hugo (and other Bootstrap enabled) themes in Hugo, when loaded as a Hugo module.
