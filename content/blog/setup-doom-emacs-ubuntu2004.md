---
title: "Setting up doom emacs in Ubuntu 20.04"
description: "meta description"
image: "images/post/setting-up-doom-emacs-ubuntu/setting-up-doom-emacs-ubuntu.jpg"
date: 2022-08-05T13:20:00+02:00
categories: ["setup", "emacs"]
tags: ["#100DaysToOffload"]
type: "regular" # available types: [featured/regular]
draft: false
---
#### Introduction

In this post I will capture the steps required to install doom emacs in a fresh new install of Ubuntu 20.04 LTS. Ubuntu was installed with the default packages.

#### Instructions

doom emacs installation instructions are missing from the current repository, so I had to look around for a new version. Fortunately Henrik, the creator of doom emacs, shared a link to the [latest version of the docs](https://github.com/doomemacs/doomemacs/blob/develop/docs/) at the doom emacs' Discord.

##### Removing previous version

By default Ubuntu 20.04 brings emacs 26.3. You need emacs 27.1 or higher to run doom emacs. So, first step is uninstall emacs 26.3. You can try to do it with the following commands:

```sh
sudo apt remove emacs
sudo apt autoremove
```

In my particular case, it didn't work. To remove it completely I had to run also:

```sh
sudo rm -f /usr/bin/emacs
```

#### Installing emacs 28.1 and the pre-requisites

Next step is installing the latest version of emacs:

```sh
sudo snap install --classic emacs
```

Then the pre-requisites:

```sh
sudo apt-get install ripgrep fd-find
```

Given I had deinstalled emacs and installed a new version (and my first snap), I could not run emacs at this moment. I checked `/etc/environment` file to find `/snap/bin` at system's PATH. I decided to close the session and launch a new one. I could launch emacs after logging again. [1]

#### Installing doom emacs

To install doom emacs, I run the following commands:

```sh
git clone https://github.com/hlissner/doom-emacs ~/.emacs.d
~/.emacs.d/bin/doom install
```

It took 16:28 minutes to compile all packages.

#### Configuring doom emacs

To open doom emacs configuration files execute `C-c f P`. This will open the .doom.d/ folder, with the following files:

- init.el: to select which doom emacs modules to enable.
- packages.el: to install melpa packages into doom emacs.
- config.el: to configure specific packages or modules.

There are also some additional options at `custom.el`, created by Custom module.

My current configuration files are stored at this [Github repository](https://github.com/elsatch/doom-emacs-config-mac).

After cloning and customizing, you need to get doom in sync using the following command:

```sh
~/.emacs.d/bin/doom sync
```

Then you can either restart doom emacs or run `M-x doom/reload`.

_Photo by Gabriel Heinzer at Unsplash_

[1]: I could have done `source /etc/environment` but... logging off felt more natural at the moment.
