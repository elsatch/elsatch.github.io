---
title: "Introduction to Organize tool"
description: "meta description"
image: "images/post/introduction-organize-tool.png"
date: 2022-02-08T14:00:00+02:00
categories: ["python", "organize-tool"]
type: "regular" # available types: [featured/regular]
draft: false
---

Organize is a command line python utility that helps you organize your files based on rules. It is created by Thomas Feldmann and it's available from his github repository. Yesterday, version 2.0 of the tool was launched. As I was reading all new features on the changelog, I began to reflect on my current use cases plus all the new options to explore.

It is my goal to use organize as much as possible to help me:

- Sorting all my pictures and folders
- Arranging paper and digital invoices and expenses
- Creating more robust backups
- Migrating all my files from Dropbox to Google Drive/local NAS

Note: In this article, we will be using the new Organize v2 syntax. Please refer to [this guide](https://organize.readthedocs.io/en/latest/updating-from-v1/) if you want to know the differences from the v1 syntax.

#### How does organize work?

Organize is a command line program that can be installed from [pip/git/brew](https://organize.readthedocs.io/en/latest/updating-from-v1/).

Once installed you can execute it from your terminal (or powershell window). The program reads the rules to process from a file, known as the organize config file. You can edit the rules executing:

```sh
organize edit
```

By default, this will open vim to edit the file, but you can use any text editor. This file's format is in YAML

Configuration file is based upon the following elements:

- Rules
- Locations
- Filters
- Actions

So each rule can target to one or more locations (folders), filtering the results in several ways and applying the actions for the filtered results. When we run organize, it will read the config file and execute the rules in the given order. Given they are the building blocks of our file organization strategy, let's explore these rules in more detail.

##### Sample rules for organizing files

There are countless ways to organize your files. Maybe you sort your files by project or by date. Maybe you prefer to rename files or tag them using macsosx built-in features. Organize rules are super flexible, trying to accommodate all kinds of scenarios.

Before we dive into the actual rules, let's describe some sorting mechanisms in plain words. For example:

- All my installation files in the download folder that are older than three months should be deleted
- All my photos in my computer should be renamed to include the date and the model of the camara that was used to take the shoots.
- All my invoices in project folders and pdf format should be copied into the fiscal_information folder, sorted by quarter, based on the content of the invoice.
- All my raw videos in the published folder should be copied to Dropbox and Google Drive for archival.

These rules can be easily replicated in Organize, using the four elements outlined in the previous section.

As an example, let's share one sample rule configuration. This rule will move all files from the Desktop folder, older than 30 days, to a folder called old_items:

```yaml
# Sample organize configuration
rules:
  - name: Move files on desktop older than 30 days to old_items
    locations: "~/Desktop"
    filters:
      - created:
          days: 30
    actions:
      - move: "~/Desktop/old_items"
```

#### Organize command-line operations

YAML files can be hard to create right without the proper editor. Fortunately, you can verify your config file by running:

```sh
organize check
```

Organize can be executed in simulation mode to verify if the rules would affect the expected files and folders. It will also return the results of the different actions, but without altering your files in any way. To run organize in simulation mode run:

```sh
organize sim
```

Once you are happy about the results, you can just process the rules doing:

```sh
organize run
```

You will see in the console all different operations happening in real time, until all rules are processed for the target locations.

#### Initial thoughts about Organize

When I first encountered organize, it looked like the perfect tool to automate renaming and moving files inside my computer. Then I started creating the rules and simulating them. So it looked like a tool for interactive sessions. Initially, it made no sense. But the more I used it, the more it grew on me.

With version 2, Organize can fulfill even more roles in my systems. In these series of articles, I plan to explore the multiple use cases for the tool, offering sample configurations and new uses that where not possible in previous versions.

_Photo credits: Photo by Erol Ahmed on Unsplash._
