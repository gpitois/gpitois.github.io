---
layout: post
title:  "The ForEach Issue in Java (and Other Languages)"
description: Debate on the use of foreach to loop over collections in Java and how they allow for weak functional programming habits.
date:   2020-03-23 21:00:54 -0700
categories: blog
---

## Background
With the addition of the stream API in Java 8, JVM programmers gained a convenient access to using Functional Programming in their source code. The new stream syntax allowed for a more natural way to express programming needs and made the code more concise. Java code bases progressively turned to a new, different look, with entire blocks of code written using a functional style in the middle of the legacy code using an imperative style. This is in general a great step forward but it also comes with some pitfalls and one of them is related to the new `forEarch()` method. Let's see why.

## What's wrong with forEach?
Among the new tools available for Java 8 developers is the `forEach()` method for iterable classes (for example for lists, a.k.a `List<E>`) and for the ubiquitous hashtable (a.k.a `Map<K,V>`). Also shipped with Java 8, the stream API was a ground breaking change for collections (which are iterable). However `forEach()` and stream are completely different tools. The stream API enables for transformation operations on collections, while `forEach()` is simply an internal iterator for collections. The problem is that their syntax _look_ similar and for this reason, developers who started writing Functional Programming Java code with streams are also tempted to use `forEach()` and its similar syntax. However `forEach()` is not a Functional Programming operator, on the contrary it encourages [side effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science)) (see example below)

{% highlight java %}
List<Integer> list = Arrays.asList(1, 2, 3);
int sum = 0;
list.forEach(item -> sum += item);
{% endhighlight %}

In this example we see that `forEach()` accepts a lambda expression as parameter, similarly to what happens with the stream API (ex: `map(x -> doSomething(x)`). In this case the lambda expression accumulates all the values of the list into the variable `sum`.
 This lambda expression has a side effect since the variable `sum` is declared beforehand and is modified while iterating on the list.

## Is it a big deal?
No, it is not a big deal but... it gave birth to a few interesting coding patterns that spread over time around the use of `forEach()`. In particular, we started to create hybrid code mixing pre-java 8 and post-java 8 worlds, along with potential misconceptions about Functional Programming. In the pre-java 8 world, it was very common to have a variable that was meant to be updated a bunch of times in order to get an aggregated information (like the sum in the previous example). In the post-java 8 world, despite the availability of functional patterns to extract the same information, we find often the use of `forEach()` to simply replace the legacy `for` loop and the use of a state variable remains.

In most of these cases the `forEach()` code block could be replaced by an actual functional formulation. Let's see a typical example.

### An Example of "mix-up"
The following code example can be commonly found on production code.

{% highlight java %}
    static class River {
        String name;
        int length;

        public River(String name, int length) {
            this.name = name;
            this.length = length;
        }
    }
    
    static class RiverHelper {
        River longest;
    }
    
    static String getLongestRiver(List<River> rivers) {
        RiverHelper riverHelper = new RiverHelper();
        rivers.forEach(river ->
            updateIfLonger(river, riverHelper));
        return riverHelper.longest.name;
    }

    static void updateIfLonger(River current, RiverHelper riverHelper) {
        if (riverHelper.longest == null) {
            riverHelper.longest = current;
        } else if (current.length > riverHelper.longest.length) {
            riverHelper.longest = current;
        }
    }
    
    public static void main(String[] args) {
        List<River> rivers = List.of(
            new River("Yangtze", 6378),
            new River("Nile", 6693),
            new River("Yenisei/Angara", 5539),
            new River("Mississippi/Misouri", 5970));
        System.out.println("The longest river in the world is: " + getLongestRiver(rivers));
    }
{% endhighlight %}
In this example the method `getLongestRiver()` uses a java 8 addition with a `forEach()` but its code does not look really different than pre-java 8 code. An experienced programmer would probably find this syntax quite heavy.

Let's try to refactor this functionality using a Functional Programming approach using the stream package.

{% highlight java %}    
    static String functionalGetLongestRiver(List<River> rivers) {
        return rivers.stream()
            .max(Comparator.comparingInt(river -> river.length))
            .map(river -> river.name)
            .orElse("No river found");
    }
    public static void main(String[] args) {
      // (...)
      System.out.println("The longest stream in the world is: " + functionalGetLongestRiver(rivers));
    }
{% endhighlight %}

As you can see, this functional formulation is more concise and reads more naturally. The Helper class is not needed anymore since we do not need to save any information.

# Conclusion
Java 8 came with a lot of great additions for developers. One of the most significant addition was the Stream API, which allowed for cleaner code when manipulating collections. The Stream API also empowered developers with the possibility of using a functional programming approach when manipulating collections. At the same time, the addition of the internal iterator `forEach` led to code that was not necessarily cleaner (at least in my opinion), and encouraged unpleasant coding patterns containing side effects. Hopefully this article helps reflect on the use of `forEach()` and you might spot a weird pattern next time you run into one of them. The fun part is that there is probably a functional alternative to it!

# Further Reading

* [https://blog.jooq.org/2015/12/08/3-reasons-why-you-shouldnt-replace-your-for-loops-by-stream-foreach/](https://blog.jooq.org/2015/12/08/3-reasons-why-you-shouldnt-replace-your-for-loops-by-stream-foreach/)
* [https://en.wikipedia.org/wiki/Side_effect_(computer_science)](https://en.wikipedia.org/wiki/Side_effect_(computer_science))
