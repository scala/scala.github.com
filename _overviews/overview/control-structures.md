---
title: Control Structures
description: This page provides an introduction to Scala's control structures, including if/then/else, 'for' loops, 'for' expressions, 'match' expressions, try/catch/finally, and 'while' loops.
---

<!--
- TODO: Introduce multiversal equality in this section?
- TODO: Should this section show (a) general control structure syntax and (b) syntax rules, such as when `do` is required? I currently assumed this should be in the Reference.
-->

Scala has the control structures you expect to find in a programming language, including:

- `if`/`then`/`else`
- `for` loops
- `while` loops
- `try`/`catch`/`finally`

It also has two other powerful constructs that you may not have seen before, depending on your programming background:

- `for` expressions (also known as _`for` comprehensions_)
- `match` expressions

These are all demonstrated in the following sections.



## The if/then/else construct

A one-line Scala `if` statement looks like this:

```scala
if x == 1 then println(x)
```

When you need to run multiple lines of code after an `if` equality comparison, use this syntax:

```scala
if x == 1 then
  println("x is 1, as you can see:")
  println(x)
```

The `if`/`else` syntax looks like this:

```scala
if x == 1 then
  println("x is 1, as you can see:")
  println(x)
else
  println("x was not 1")
```

And this is the `if`/`else if`/`else` syntax:

```scala
if x < 0 then
  println("negative")
else if x == 0
  println("zero")
else
  println("positive")
```

You can optionally include an `end if` statement at the end of each expression, if you prefer:

```scala
if x == 1 then
  println("x is 1, as you can see:")
  println(x)
end if
```


### `if`/`else` expressions always return a result

Note that `if`/`else` comparisons form _expressions_, meaning that they return a value which you can assign to a variable. Because of this, there’s no need for a special ternary operator:

```scala
val minValue = if a < b then a else b
```

Because they return a value, you can use `if`/`else` expressions as the body of a method:

```scala
def compare(a: Int, b: Int): Int =
  if a < b then
    -1
  else if a == b then
    0
  else
    1
```

### Aside: Expression-oriented programming

As a brief note about programming in general, when every expression you write returns a value, that style is referred to as _expression-oriented programming_, or EOP. For example, this is an _expression_:

```scala
val minValue = if a < b then a else b
```

Conversely, lines of code that don’t return values are called _statements_, and they’re used for their _side-effects_. For example, these lines of code don’t return values, so they’re used for their side effects:

```scala
if a == b then doSomething()
println("Hello")
```

The first example runs the `doSomething` method as a side effect when `a` is equal to `b`. The second example is used for the side effect of printing a string to STDOUT. As you learn more about Scala you’ll find yourself writing more _expressions_ and fewer _statements_.



## `for` loops

In its most simple use, a Scala `for` loop can be used to iterate over the elements in a collection. For example, given a sequence of integers, You can loop over its elements and print their values like this:

```scala
val ints = Seq(1,2,3)
for i <- ints do println(i)
```

The code `i <- ints` is referred to as a _generator_, and if you leave the parentheses off of the generator, the `do` keyword is required before the code that follows it. Otherwise you can write the code like this:

```scala
for (i <- ints) println(i)
```

Regardless of which approach you use, this is what the result looks like in the Scala REPL:

````
scala> val ints = Seq(1,2,3)
ints: Seq[Int] = List(1, 2, 3)

scala> for i <- ints do println(i)
1
2
3
````

When you need a multiline block of code following the `if` condition, use either of these approaches:

```scala
// option 1
for
  i <- ints
do
  val x = i * 2
  println(s"i = $i, x = $x")

// option 2
for (i <- ints) 
  val x = i * 2
  println(s"i = $i, x = $x")

// option 3
for (i <- ints) {
  val x = i * 2
  println(s"i = $i, x = $x")
}
```

### Multiple generators

`for` loops can have multiple generators, as shown in this example:

```scala
for
  i <- 1 to 2
  j <- 'a' to 'b'
  k <- 1 to 10 by 5
do
  println(s"i = $i, j = $j, k = $k")
```

That expression prints this output:

````
i = 1, j = a, k = 1
i = 1, j = a, k = 6
i = 1, j = b, k = 1
i = 1, j = b, k = 6
i = 2, j = a, k = 1
i = 2, j = a, k = 6
i = 2, j = b, k = 1
i = 2, j = b, k = 6
````

### Guards

`for` loops can also contain `if` statements, which are known as _guards_:

```scala
for
  i <- 1 to 5
  if i % 2 == 0
do
  println(i)
```

The output of that loop is:

````
2
4
````

A `for` loop can have as many guards as needed. This example shows one way to print the number `4`:

```scala
for
  i <- 1 to 10
  if i > 3
  if i < 6
  if i % 2 == 0
do
  println(i)
```

### Using `for` with Maps

You can also use `for` loops with a `Map`. For example, given this `Map` of movie names and ratings:

<!-- TODO: use a different example -->
```scala
val ratings = Map(
  "Lady in the Water"  -> 3.0, 
  "Snakes on a Plane"  -> 4.0, 
  "You, Me and Dupree" -> 3.5
)
```

You can print the movie names and ratings using `for` like this:

```scala
for (name,rating) <- ratings do println(s"Movie: $name, Rating: $rating")
```

Here’s what that looks like in the REPL:

```scala
scala> for (name,rating) <- ratings do println(s"Movie: $name, Rating: $rating")
Movie: Lady in the Water, Rating: 3.0
Movie: Snakes on a Plane, Rating: 4.0
Movie: You, Me and Dupree, Rating: 3.5
```

As the `for` loop iterates over the map, each rating is bound to the variables `name` and `rating`, which are in a tuple:

```scala
(name,rating) <- ratings
```

As the loop runs, the variable `name` is assigned to the current _key_ in the map, and the variable `rating` is assigned to the current map _value_.

<!-- TODO: show other possible uses/styles here?
for (name,rating) <- ratings
do println(s"Movie: $name, Rating: $rating")

for
  (name,rating) <- ratings
do
  println(s"Movie: $name, Rating: $rating")

for (name,rating) <- ratings do { println(s"Movie: $name, Rating: $rating") }
for ((name,rating) <- ratings) println(s"Movie: $name, Rating: $rating")
-->



## `for` expressions

In the previous `for` loop examples, those loops were all used for _side effects_, specifically to print those values to STDOUT using `println`.

It’s important to know that you can also create `for` _expressions_ that return values. You create a `for` expression by adding the `yield` keyword and an expression to return, like this:

```scala
val list = 
  for
    i <- 10 to 12
  yield
    i * 2

// result: list == Vector(20,22,24)
```

After that `for` expression runs, the variable `list` is a `Vector` that contains the values shown. This is how the expression works:

1. The `for` expression starts to iterate over the values in the range `(10, 11, 12)`. It first works on the value `10`, multiplies it by `2`, then _yields_ that result, the value `20`.
2. Next, it works on the `11` — the second value in the range. It multiples it by `2`, then yields the value `22`. You can think of these yielded values as accumulating in a temporary holding place.
3. Finally the loop gets the number `12` from the range, multiplies it by `2`, yielding the number `24`. The loop completes at this point and yields the final result, the `Vector(20,22,24)`.

`for` expressions can be used any time you need to traverse all of the elements in a collection and apply an algorithm to those elements to create a new list.

Here’s an example that shows how to use a block of code after the `yield`:

```scala
val names = List("_adam", "_david", "_frank")

val capNames = for name <- names yield
  // imagine this algorithm requires multiple lines
  val nameWithoutUnderscore = name.drop(1)
  val capName = nameWithoutUnderscore.capitalize
  capName

// result: List(Adam, David, Frank)
```


### Using a `for` expression as the body of a method

Because a `for` expression yields a result, it can be used as the body of a method that returns a useful value. This method returns all of the values in a given list of integers that are between `3` and `10`:

```scala
def between3and10(xs: List[Int]): List[Int] = 
  for
    x <- xs
    if x >= 3
    if x <= 10
  yield x

between3and10(List(1,3,7,11))   // result: List(3, 7)
```

<!-- TODO: add a summary?
- they are commonly used
- a for-expression without a guard is the same as `map()`
-->



## `while` loops

Scala `while` loop syntax looks like this:

```scala
var i = 0

while i < 3 do
  println(i)
  i += 1
```

If you use parentheses around the test condition, it can also be written like this:

```scala
var i = 0

while (i < 3) {
  println(i)
  i += 1
}
```

<!-- NOTE: I had a do/while example here, but it’s used so rarely I didn’t think it should be included in an overview. -->



## `match` expressions

Pattern matching is a major feature of functional programming languages, and Scala includes a `match` expression that has many capabilities.

In the most simple case you can use a `match` expression like a Java `switch` statement, matching cases based on an integer value. Notice that this really is an expression, as it evaluates to a result:

```scala
import scala.annotation.switch

// `i` is an integer
val day = i match
  case 0 => "Sunday"
  case 1 => "Monday"
  case 2 => "Tuesday"
  case 3 => "Wednesday"
  case 4 => "Thursday"
  case 5 => "Friday"
  case 6 => "Saturday"
  case _ => "invalid day"   // the default, catch-all
```

In this example the variable `i` is tested against the cases shown. If it’s between `0` and `6`, `day` is bound to a string that represents one of the days of the week. Otherwise, the catch-all case is represented by the `_` character, and `day` is bound to the string, `"invalid day"`.

>When writing simple `match` expressions like this, it’s recommended to use the `@switch` annotation on the variable `i`. This annotation provides a compile time warning if the switch can’t be compiled to a `tableswitch` or `lookupswitch`, which are better for performance. See the Reference documentation for more details.


### Using the default value

When you need to access the catch-all, default value in a `match` expression, just provide a variable name on the left side of the `case` statement, and then use that variable name on the right side of the statement as needed:

```scala
i match
  case 0 => println("1")
  case 1 => println("2")
  case what => println(s"You gave me: $what" )
```

In this example the variable is named `what` to show that it can be given any legal name, but it’s more commonly given a name like `default`.


### Handling multiple possible matches on one line

As mentioned, `match` expressions have many capabilities. This example shows how to use multiple possible pattern matches in each `case` statement:

```scala
val evenOrOdd = i match
  case 1 | 3 | 5 | 7 | 9 => println("odd")
  case 2 | 4 | 6 | 8 | 10 => println("even")
  case _ => println("some other number")
```


### Using `if` expressions in `case` statements

You can also use `if` expressions in the `case` statements of `match` expressions. In this example the second and third `case` statements both use `if` expressions to match multiple integer values:

```scala
i match
  case 1 => println("one, a lonely number")
  case x if x == 2 || x == 3 => println("two’s company, three’s a crowd")
  case x if x > 3 => println("4+, that’s a party")
  case _ => println("i’m guessing your number is zero or less")
```

Here’s another example that shows how you can use `if` expressions in `case` statements. This example shows how to match a given value against ranges of numbers:

```scala
i match
  case a if 0 to 9 contains a => println(s"0-9 range: $a")
  case b if 10 to 19 contains b => println(s"10-19 range: $b")
  case c if 20 to 29 contains c => println(s"20-29 range: $c")
  case _ => println("Hmmm...")
```


#### Case classes and match expressions

You can also extract fields from `case` classes — and classes that have properly written `apply`/`unapply` methods — and use those in your guard conditions. Here’s an example using a simple `Person` case class:

```scala
case class Person(name: String)

def speak(p: Person) = p match
  case Person(name) if name == "Fred" => println(s"$name says, Yubba dubba doo")
  case Person(name) if name == "Bam Bam" => println(s"$name says, Bam bam!")
  case _ => println("Watch the Flintstones!")

speak(Person("Fred"))      // "Fred says, Yubba dubba doo"
speak(Person("Bam Bam"))   // "Bam Bam says, Bam bam!"
```


### Using a `match` expression as the body of a method

Because `match` expressions return a value, they can be used as the body of a method. This method takes a `Boolean` value as an input parameter, and returns a `String`, based on the result of the `match` expression:

```scala
def isTrue(a: Any) = a match
  case 0 | "" => false
  case _ => true
```

Because the input parameter `a` is defined to be the `Any` type — which is the root of all Scala classes, like `Object` in Java — this method works with any data type that’s passed in. After that, the body of the method is just two `case` statements, one that equates `0` or an empty string to `false`, and a default case that returns `true` for any other value. These examples show how this method works:

```scala
isTrue(0)      // false
isTrue("")     // false
isTrue(1)      // true
isTrue(" ")    // true
isTrue(2F)     // true
```

Using a `match` expression as the body of a method is a very common use.


#### Match expressions support many different types of patterns

`match` expressions can work with many different types of patterns. The pattern types that can be matched are:

- Constant
- Sequence
- Tuple
- Constructor
- Typed
- Default/wildcard

All of these patterns are shown in the following `pattern` method, which takes an input parameter of type `Any` and returns a `String`:

```scala
def pattern(x: Any): String = x match

  // constant patterns
  case 0 => "zero"
  case true => "true"
  case "hello" => "you said 'hello'"
  case Nil => "an empty List"

  // sequence patterns
  case List(0, _, _) => "a 3-element list with 0 as the first element"
  case List(1, _*) => "list, starts with 1, has any number of elements"
  case Vector(1, _*) => "vector, starts w/ 1, has any number of elements"

  // tuples
  case (a, b) => s"got $a and $b"
  case (a, b, c) => s"got $a, $b, and $c"

  // constructor patterns
  case Person(first, "Alexander") => s"Alexander, first name = $first"
  case Dog("Zeus") => "found a dog named Zeus"

  // typed patterns
  case s: String => s"got a string: $s"
  case i: Int => s"got an int: $i"
  case f: Float => s"got a float: $f"
  case a: Array[Int] => s"array of int: ${a.mkString(",")}"
  case as: Array[String] => s"string array: ${as.mkString(",")}"
  case d: Dog => s"dog: ${d.name}"
  case list: List[_] => s"got a List: $list"
  case m: Map[_, _] => m.toString

  // the default wildcard pattern
  case _ => "Unknown"
```

`match` expressions and patterns are discussed in more detail in the Reference documentation.



## try/catch/finally

Like Java, Scala has a `try`/`catch`/`finally` construct to let you catch and manage exceptions. For consistency, Scala uses the same syntax that `match` expressions use: `case` statements to match the different possible exceptions that can occur.

Here’s an example of Scala’s `try`/`catch`/`finally` syntax. In this example, `openAndReadAFile` is a method that does what its name implies: it opens a file and reads the text in it, assigning the result to the mutable variable `text`:

```scala
var text = ""
try
  text = openAndReadAFile(filename)
catch
  case fnf: FileNotFoundException => fnf.printStackTrace()
  case ioe: IOException => ioe.printStackTrace()
finally
  // close your resources here
  println("Came to the 'finally' clause.")
```

Assuming that the `openAndReadAFile` method uses the Java _java.io.*_ classes to read a file and doesn’t catch its exceptions, attempting to open and read a file can result in both a `FileNotFoundException` and an `IOException`, and those two exceptions are caught in the `catch` block of this example.



## More information

That covers the basics of Scala control structures. For more details, see the Reference documentation.




