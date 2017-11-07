# Scala CheatSheet

## Useful Links
- [The official Cheatsheet](http://docs.scala-lang.org/cheatsheets/index.html)
- [Tour of scala](https://docs.scala-lang.org/tour/tour-of-scala.html)
- [Scala Cookbook](http://scalacookbook.com/)
- [Scala Cookbook 2] (https://alvinalexander.com/scala/scala-programming-cookbook-recipes-faqs)

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

## implicit 

[Implicit for typeclasses](https://www.cakesolutions.net/teamblogs/demystifying-implicits-and-typeclasses-in-scala)

### parameter
The actual arguments that are eligible to be passed to an implicit parameter fall into two categories:

First, eligible are all identifiers x that can be accessed at the point of the method call without a prefix and that denote an implicit definition or an implicit parameter.
Second, eligible are also all members of companion modules of the implicit parameter’s type that are labeled implicit.

```scala
// simple example
// probably in a library
class Prefixer(val prefix: String)
def addPrefix(s: String)(implicit p: Prefixer) = p.prefix + s

//then probably in your application
implicit val myImplicitPrefixer = new Prefixer("***")
addPrefix("abc")  // returns "***abc"

```

```scala
//more complex one
abstract class SemiGroup[A] {
  def add(x: A, y: A): A
}
abstract class Monoid[A] extends SemiGroup[A] {
  def unit: A
}
object ImplicitTest extends App {
  implicit object StringMonoid extends Monoid[String] {
    def add(x: String, y: String): String = x concat y
    def unit: String = ""
  }
  implicit object IntMonoid extends Monoid[Int] {
    def add(x: Int, y: Int): Int = x + y
    def unit: Int = 0
  }
  def sum[A](xs: List[A])(implicit m: Monoid[A]): A =
    if (xs.isEmpty) m.unit
    else m.add(xs.head, sum(xs.tail))

  println(sum(List(1, 2, 3)))       // uses IntMonoid implicitly
  println(sum(List("a", "b", "c"))) // uses StringMonoid implicitly
}
```

### conversion

```scala
implicit def doubleToInt(d: Double) = d.toInt
val x: Int = 42.0
```


## collections

```scala
// head, tail, isEmpty
object Demo {
   def main(args: Array[String]) {
      val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
      val nums = Nil

      println( "Head of fruit : " + fruit.head )
      println( "Tail of fruit : " + fruit.tail )
      println( "Check if fruit is empty : " + fruit.isEmpty )
      println( "Check if nums is empty : " + nums.isEmpty )
   }
}

//:::
val fruit1 = "apples" :: ("oranges" :: ("pears" :: Nil))
val fruit2 = "mangoes" :: ("banana" :: Nil)

// use two or more lists with ::: operator
var fruit = fruit1 ::: fruit2 //List(apples, oranges, pears, mangoes, banana)

// fill 
val fruit = List.fill(3)("apples") // Repeats apples three times.

// reverse

val fruit = "apples" :: ("oranges" :: ("pears" :: Nil))
fruit.reverse

// pattern matching

def listToString(list: List[String]): String = list match {
    case s :: rest => s + " " + listToString(rest)
    case Nil => ""
}

```



## Option, Some and None

```scala
//example 1
def toInt(in: String): Option[Int] = {
    try {
        Some(Integer.parseInt(in.trim))
    } catch {
        case e: NumberFormatException => None
    }
}
val bag = List("1", "2", "foo", "3", "bar")
val sum = bag.flatMap(toInt).sum
```

## Error handling with Option or Either

### with just Option[T]
```scala

case class Person(name: String, age: Int)

def validateName(name: String): Option[String] = {
    if (name.isEmpty) None
    else Some(name)
}

def validateAge(age: Int): Option[Int] = {
    Some(age)
}

val p = for {
    name <- validateName("Matt")
    age <- validateAge(30)
} yield Person(name, age)

println(p) //print Option[Person(Matt, 30)]
```

### with Either[L, R]
```scala
case class Person(name: String, age: Int)

object Hello extends Greeting with App {

  def validateName(name: String): Either[String, String] = {
    if (name.isEmpty) Left("Name cannot be empty")
    else Right(name)
  }

  def validateAge(age: Int): Either[String, Int] = {
    Right(age)
  }

  val p = for {
    name <- validateName("")
    age <- validateAge(30)
  } yield Person(name, age)

  println(p)

}
```