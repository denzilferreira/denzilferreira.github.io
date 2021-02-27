---
title: Refresher on Android development
author: Denzil Ferreira
date: '2021-02-21'
slug: []
categories:
  - Software Dev
tags:
  - android
  - mobile
  - dev
Description: ''
Tags: [Android]
Categories: [Developer]
DisableComments: no
---

# Preface

I've been developing Android applications for research since 2010. I started on Android 1.0 (using the HTC Dream) and witnessed changes unfold at a rapid pace. When I had time to tinker, I kept mostly up to date with the source code of AOSP, daring to compile from scratch my own phone firmware. You can find some of my posts at XDA-Developers' forums, back from when I had an HTC Magic, Nexus One, OnePlus and even Mobile Phone 6.5 series :smile: In September, I joined [Solita Oy](https://www.solita.fi) to create and build mobile solutions. Since then, I had to revisit what changed on Android. To no-one's surprise, my experience with Android needed updating. Here you'll find a summary of the new goodies on Android development.

## Kotlin

It is an understatement to say that [Kotlin](https://kotl.in) is just another alternative language. Kotlin is a very powerful, yet stupidly simple language to learn.

{{<mermaid>}}
graph TB
A(JVM Runtime) --> B[Java Heap]
A(JVM Runtime) --> C[Class loading]
A(JVM Runtime) --> D[JIT Compilation]
A(JVM Runtime) --> E[Java Threads]
{{</mermaid>}}