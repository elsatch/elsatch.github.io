---
title: "Creating custom shortcuts for apps in MacOS"
description: "meta description"
image: "images/post/creating-custom-shortcuts-for-apps-in-macos/creating-custom-shortcuts-for-apps-in-macos.png"
date: 2022-03-23T15:00:00+02:00
categories: ["macos", "workflow"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
#### Introduction
There are several tasks that you repeat over and over in your desktop. Wouldn't it be great to be able to launch them as fast a possible? There are dedicated apps to launch apps, like the wonderful Alfred launcher. But what if you just want to click an item in the menu or an icon on the interface? Fortunately, MacOS has built-in capabilities for that kind of workflow.

#### Motivation
I am using Drafts app to capture contents both on the go and on my desktop computer. They act as an temporary inbox before I process them. Drafts integrates with Safari share menu. So I can highlight some text and click the Share->Drafts menu. A new window pop-ups, saving the URL, the highlighted region and asks for additional notes or information before saving. I can save it pressing Cmd+Enter.
![Share in Drafts button](images/post/creating-custom-shortcuts-for-apps-in-macos/share-in-drafts-button.png)

All these notes are tagged as 'captured-from-desktop', so I can filter them afterwards easily using the workspaces features in Drafts.

To streamline this whole process, how can I launch the Share action in Safari without having to click the interface?

#### Creating custom shortcuts in MacOS
MacOS offers the feature to [create custom shortcuts out of the box](https://support.apple.com/en-us/guide/mac-help/mchlp2271/mac). To do so, you need to open 'System Preferences-> Keyboard -> Shortcuts'.

Besides system wide shortcuts, you can also create per app shorcuts. To do so, click on the left pane the 'App Shortcuts' element. Press the '+' sign an select Safari on the dropdown menu.

According to the manual, we need to pass as a parameter the complete route to the option we want to bind to the shortcut. But what is the route to our button in the interface?

[This thread on Drafts discourse](https://forums.getdrafts.com/t/capture-quotes-from-webpages-with-refering-urls/6727/18) offered the solution. All the share buttons are also available vía File menu. In our particular example, the complete route is 'File->Share->Drafts'. 

![App shortcuts](images/post/creating-custom-shortcuts-for-apps-in-macos/app-shortcuts.png)

>Note that this route is language dependant. For example, in Spanish MacOS route is 'Archivo->Compartir->Drafts' 

__Cover image Photo by Andrii Leonov on Unsplash._
  
