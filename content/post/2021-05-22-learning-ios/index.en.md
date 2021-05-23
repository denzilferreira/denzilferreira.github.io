---
title: Learning Swift
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

My notes while studying Swift and iOS programming. Some of these are familiar and shared in modern languages.

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
password="1234"
print(password!) //print the assigned value of the password. 
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

for index in 1...10 { //In Kotlin we use ..
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

> To stop a loop: **break** and to skip an iteration of a loop: **continue**

## Array, Set, Dictionary

#### Array

A collection of items.

``` Swift
var items = [1,2,3,4] //array of Int
for item in items {
  print("Item: \(item)")
}

//Swift is 0-indexed. e.g., modify the second (index = 1) element to 5.
items[1] = 5

//e.g., an empty mutable array of Int
var items = [Int]()
items.append(1) //items contains [1]
items.append(2) //items contains [1,2]

//e.g., an empty mutable array of any type
var mixed = [Any]()
mixed.append(1)
mixed.append("Hello") //mixed contains [1,"Hello"]
```

#### Set

A collection of unique items.

``` Swift
var elements = Set<String>()
elements.insert("Alice")
elements.insert("Bob")
elements.insert("Bob")
```
> **elements** will only contain **Alice and Bob**, even thought we inserted Bob twice.

#### Dictionary

A key-indexable collection of items.

``` Swift
var coins = [String:String]
elements["EUR"] = "Euro"
elements["USD"] = "US Dollar"
print(elements["EUR"]!) //prints "Euro"
```

## Functions

``` Swift
func sum(number1:Int, number2:Int) -> Int {
  return number1 + number2
}
```