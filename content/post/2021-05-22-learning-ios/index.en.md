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

> Note: to access the value of __password__ above, you need to use **!**, e.g.:

``` Swift
print(password!) //print the assigned value of the password. 

//Multiple lines comments look like this
/*  
  In this case, it will be give you an error 
  because it's a String? and has no value
*/
```

Constants are defined with __let__:

``` Swift
let userId = 123456 //can't reassign the variable userId
```

## Printing

``` Swift
print("Hello, world!")
```

## Casting

Converting a variable type to another looks like this:

``` Swift
//convert String to Int
var aText:String = "123"
var aNumber = Int(aText) //this is an Int

//convert Int to String
var intValue = 123
var strValue = String(intValue)

//convert Double to Int
var doubleNumber:Double = 1.2
var intDouble = Int(doubleNumber) //this will be 1 - we lose precision
```

## For, while, repeat

``` Swift
//Different ways to count from 1 to 10

for index in 1...10 {
  print("Number: \(index)")
}

var counter = 1
while counter <= 10 {
  print("Number: \(counter)")
  counter = counter + 1
}

counter = 1
repeat {
  print("Number: \(counter)")
  counter = counter + 1
} while count <= 10
```

> To stop a loop: **break**;
> To skip a loop: **continue**

