---
title: Learning Flutter
author: Denzil Ferreira
date: '2021-03-06'
slug: []
categories: 
  - Flutter
tags: 
  - learning
  - flutter
  - cross-platform
Description: ''
Tags: [Flutter, Cross-platform]
Categories: [Learning]
DisableComments: no
---

# What is Flutter?

[Flutter](https://flutter.dev/) allows you to build **native** cross-platform (iOS, Android, web, Windows, macOS, Linux) apps with one programming language and codebase. In a nutshell, it is an SDK & Framework bundle:

{{<mermaid>}}
graph TD
    A[SDK Software Development Kit] --> B[Tools to compile code to native machine + ease of code]
    C[Framework/Widget Library] --> D[Re-usable UI = widgets, utility functions, packages]
{{</mermaid>}}

The one programming language we need to use is [Dart](https://dart.dev/), which is focused on frontend user interfaces (UIs) development. It is an object-oriented and strongly typed language. As a language, Dart is familiar to those with Javascript or Java background.

One may be tempted to assume then that Flutter is only for making interfaces. Far from that. Creating a UI in Flutter is not done using a [WUSIWYG](https://en.wikipedia.org/wiki/WYSIWYG) editor. Thanks to the hot-reload capabilities of the SDK, you see your changes reflected as soon as you save your source code and Flutter's moto is "UI as Code". We build our UI as a tree, starting with a root widget and add children widget inside and so on.