---
layout: post
title: "Functional Programming with Scala"
categories: programming functional
comments: false
---

Welcome! While not a new topic at all, I thought I'd schpill about
functional programming today. If you *haven't* dove into the FP
world yet, now is your chance to *drink the kool-aid* so to speak!
In this post, you'll find the most common actions used in FP;
for ease of readability and due to my longer
experience with it, I'm using the [Scala](https://www.scala-lang.org/)
language in the examples.  

---

## The Basics: map, filter, reduce
You gotta walk before you can run. Here's a quick run-down of the basic
functional programming paradigms.

```scala
val names: Seq[String] = Seq("Mark", "Ahmed", "Samantha", "Rachel", "Demetry")

// Convert all the names to uppercase
val upperCasedNames: Seq[String] = names.map { name => name.toUpperCase() }
println(upperCasedNames.mkString(", ")) // MARK, AHMED, SAMANTHA, RACHEL, DEMETRY

// Filter for names longer than 5 characters
val longNames: Seq[String] = names.filter { name => name.length > 5 }
println(longNames.mkString(", ")) // Samathna, Rachel, Demetry

// There's also a negated version of filter for convenience
val shortNames: Seq[String] = names.filterNot { name => name.length > 5 }
println(shortNames.mkString(", ")) // Mark, Ahmed

// Alternative to mkString
val joinedNames: String = names.reduce((left, right) => left + ", " + right)
println(joinedNames) // Mark, Ahmed, Samantha, Rachel, Demetry
```

That's pretty straightforward. The general use cases for these are:  
1. `map` - used for *transforming* each element
2. `filter` - used for sifting through the collection with a condition
3. `reduce` - used to join the elements into one (of the same type)

---

## Extending the Basics: flatMap, count, reduceLeft, and foldRight
```scala
// Using flatMap to only get the names with 2 or more vowels
// this example would be more appropriate with filter but alas...
val vowels: Seq[Char] = Seq('A', 'E', 'I', 'O', 'U', 'a', 'e', 'i', 'o', 'u')
val namesWithManyVowels: Seq[String] = names.flatMap { name =>
  val numVowels: Int = name.count(vowels.contains)
  if (numVowels >= 2) {
    Some(name.toUpperCase())
  } else {
    None
  }
}
println(namesWithManyVowels.mkString(", ")) // AHMED, SAMANTHA, RACHEL, DEMETRY

// Get the number of vowels in the names
val vowelCounts: Seq[Int] = names.map(_.count(vowels.contains(_)))
println(vowelCounts.mkString(", ")) // 1, 2, 3, 2, 2

// Do some weird math on it ((((1 - 2) - 3) - 2) - 2)
vowelCounts.reduceLeft(_ - _) // -8

// Combine the names into a single buffer (array of bytes), in reverse order
val reverseBuffer: Array[Byte] = names.foldRight(Array.empty[Byte])((n1, arr) => arr ++ n1.getBytes)
println(new String(reverseBuffer, "UTF-8")) // DemetryRachelSamanthaAhmedMark
```

1. `flatMap` - throws out `None` values; convenient for when you want to
do a transformation but only care about certain elements; could also be done
by combining `filter` and `map`
2. `count` - pass a closure that returns true or false to act as the condition
and get the number of true values; useful for counting things in underlying objects
3. `reduceLeft` - uses tail recursion, which will never incur a stack overflow,
to combine all the elements into one, using your closure for joining logic
4. `foldRight` - using an initial value, or *accumulator*, fold up the collection
using the closure you provide; returns the accumulator once collection is traversed

{% include alert.html content="<b>Note</b> the `reduce` and `fold` families of actions
are very similar to one another in functional programming; the difference has to do
with the return type and the initial accumulator; `reduce` returns the same type as is
in the collection, and uses the first elemnt in the collection as the initial accumulator value;
however, `fold` can return whatever type you specify as the accumulator type, and
the accumulator has whatever initial value you specify (i.e. an empty array of bytes)" level="info" %}

---

## Others: groupBy, sortWith, partition
```scala
val ages: Seq[Int] = Seq(22, 28, 19, 37, 40)

val people: Seq[Map[String, Any]] = names.zip(ages).map { case (name: String, age: Int) =>
 Map("name" -> name, "age" -> age)
}

// Group by their age group (20's, 30's, etc.)
val ageGroups: Map[Int, Seq[Map[String, Any]]] = people.groupBy(p => p("age").asInstanceOf[Int] / 10 * 10)
println(ageGroups(20).mkString(", ")) // Map(name -> Mark, age -> 22), Map(name -> Ahmed, age -> 28)

// Custom sorting; sort by age youngest to oldest
val sortedPeople: Seq[Map[String, Any]] = people.sortWith { (p1, p2) =>
  p1("age").asInstanceOf[Int] < p2("age").asInstanceOf[Int]
}
println(sortedPeople.mkString("\n"))
// Outputs:
// Map(name -> Samantha, age -> 19)
// Map(name -> Mark, age -> 22)
// Map(name -> Ahmed, age -> 28)
// Map(name -> Rachel, age -> 37)
// Map(name -> Demetry, age -> 40)

// Partition, or break down the collection into two groups
// using a condition closure
val split25: (Seq[Map[String, Any]], Seq[Map[String, Any]]) = people.partition(_("age").asInstanceOf[Int] > 25)

// Over 25
println(split25._1.mkString("\n"))
// Map(name -> Ahmed, age -> 28)
// Map(name -> Rachel, age -> 37)
// Map(name -> Demetry, age -> 40)

// Under 25
println(split25._2.mkString("\n"))
// Map(name -> Mark, age -> 22)
// Map(name -> Samantha, age -> 19)
```
