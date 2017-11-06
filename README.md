# Scala CheatSheet

[The official Cheatsheet](http://docs.scala-lang.org/cheatsheets/index.html)

## import

packages imported by default
- java.lang._
- scala._
- scala.Predef._

Basic import
```scala
// import one class
import java.io.File

// import every class in a package
import java.io._

// import multiple classes from a package (version 1)
import java.io.{File, IOException, FileNotFoundException}

// import multiple classes from a package (version 2)
import java.io.File
import java.io.FileNotFoundException
import java.io.IOException

```

Creating alias
```scala
import java.util.{List => UtilList}
import java.awt.{List => AwtList}

// later ...
val list = new AwtList
```

Hiding classes during import
```scala
//import everthing except Random
import java.util.{Random => _, _}
```

Like Java static import? 
```scala
//import all methods from java.lang.Math
import java.lang.Math._
```

## Packaging

The Java way
```scala
package foo.bar.baz
class Foo {
    override def toString = "I'm foo.bar.baz.Foo"
}
```

Curly brace style packaging
```scala
// a single package
package orderentry {
  class Foo { override def toString = "I am orderentry.Foo" }
}

// one package nested inside the other
package customers {
  class Foo { override def toString = "I am customers.Foo" }

  package database {
    // this Foo is different than customers.Foo or orderentry.Foo
    class Foo { override def toString = "I am customers.database.Foo" }
  }
}

object PackageTests extends App {
  println(new orderentry.Foo)
  println(new customers.Foo)
  println(new customers.database.Foo)
}
```

Nesting packages
```scala
// example 1
package foo {
  package bar {
    package baz {
      class Foo {
        override def toString = "I'm foo.bar.baz.Foo"
      }
    }
  }
}

// example 2

package com.alvinalexander.foo {
 class Foo { override def toString = "I am com.alvinalexander.foo.Foo" }  
}

```



## control constructs

```scala

//if expression
 if (check) happy else sad
 if (check) happy
 if (check happy else ()

 //while
var i = 0
while (i < array.length) {
  println(array(i)
  i += 1
}

//simple for loop
for (arg <- args) println(arg)

// "x to y" syntax
for (i <- 0 to 5) println(i)

// "x to y by" syntax
for (i <- 0 to 10 by 2) println(i)

// for with guard
for (x <- xs if ( x%2 == 0) yield x*10

//cross product
for (x <- xs; y <- ys) yield x*y


```


## pattern matching
```scala

//List
(xs zip ys) map { case (x,y) => x * y }

//Option
val v42 = 42
Some(3) match {
  case Some(`v42`) => println("42")
  case _ => println("Not 42")
}

val UppercaseVal = 42
Some(3) match {
  case Some(UppercaseVal) => println("42")
  case _ => println("Not 42")
}

```

## case class
```scala

// simple example
case class Book(isbn: String)
val aBook = Book("123-478787944")

// another exmaple
case class Message(sender: String, recipient: String, body: String)
val message1 = Message("abc@gmail.com", "xyz@outlook.com", "hello hello")

println(message1.sender) //prints "abc@gmail"
message1.sender = "abc1@gmail.com" //compile error

// comparison

val message2 = Message("jorge@catalonia.es", "guillaume@quebec.ca", "Com va?")
val message3 = Message("jorge@catalonia.es", "guillaume@quebec.ca", "Com va?")
val messagesAreTheSame = message2 == message3  // true


```


## functions 

```scala

//define a function
//way 1
def f(x: Int) = { x * x}

//way 2, anonymous funciton 
val f = (x: Int) => x*x

//use functions
(1 to 5).map(2*)

def sum(args: List[Int]) = args.reduceLeft(_+_)
sum(List(1,2,3,4,5)

```