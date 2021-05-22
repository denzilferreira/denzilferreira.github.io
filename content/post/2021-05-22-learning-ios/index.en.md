---
title: Learning iOS
author: Denzil ferreira
date: '2021-05-22'
slug: []
categories:
  - iOS
tags:
  - learning
  - mobile
  - iOS
---

My notes while studying Swift and iOS programming.

# Swift

Swift is the recommended language to develop for Apple devices. It is very similar to Kotlin (for Android) in many ways. So if you already know Kotlin, Swift is easy.

## Variables and Values

Swift supports **Type Inference**. For example, if I create a variable and assign it a text, it becomes a String. Example of primitive types in Swift:

- **Bool**
- **String**
- **Int**
- **Long**
- **Double**
- **Array<?>** where ? is another type
- **Dictionary<String, Int>** can be seen as a String indexed list
- **Any** a generic type, akin of Object in Java and Any in Kotlin

For example, you can do:
``` Swift
var hello = "Hello, world!"
var age:Int = 39
var password:String? //default nil, i.e., null in Apple jargon
```

Note: to access the value of __password__ above, you need to use **!**, e.g.:

``` Swift
print(password!) //print the assigned value of the password. 
/**
In this case, it will be give you an error 
because it's a String? and has no value
**/
```

Constants are defined with __let__:

``` Swift
let userId = 123456 //can't change userId to something else
```

## Printing

``` Swift
print("Hello, world!")
```
