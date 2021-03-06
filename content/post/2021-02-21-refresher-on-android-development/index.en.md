---
title: Android Develoment in 2021
author: Denzil Ferreira
date: '2021-02-21'
slug: []
categories:
  - Android
tags:
  - android
  - mobile
  - dev
Description: ''
Tags: [Android, Kotlin, Components]
Categories: [Developer]
DisableComments: no
---

# Preface

I've been developing Android applications for research since 2010. I started on Android 1.0 (using the HTC Dream) and witnessed changes unfold at a rapid pace. When I had time to tinker, I kept mostly up to date with the source code of AOSP, daring to compile from scratch my own phone firmware. You can find some of my posts at XDA-Developers' forums, back from when I had an HTC Magic, Nexus One, OnePlus and even Mobile Phone 6.5 series under the user "dferreira" :smile: In September, I joined [Solita Oy](https://www.solita.fi) to create and build mobile solutions. Since then, I had to revisit what changed on Android. To no-one's surprise, my experience with Android needed updating. Here you'll find a summary of the new goodies on Android development.

## Kotlin

It is an understatement to say that [Kotlin](https://kotl.in) is just another alternative language. Kotlin is a very powerful, yet stupidly simple language to learn. The traditional "Hello World" would look like this:

``` kotlin
fun main() {
    println("Hello World")
}
```

What is great about Kotlin is that it runs on the JVM and is 100% compatible with Java's legacy: i.e., UNLIMITED POWER :muscle: