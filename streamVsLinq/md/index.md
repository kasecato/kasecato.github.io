# Stream vs. LINQ

[<img style="border-radius: 50% !important;" src="img/twitter.jpg" width="200px" alt="Keisuke KATO">](https://twitter.com/k_kato) 

April 8, 2016 @k_kato




# About me

<!-- .slide: data-background="rgb(0, 43, 54)" -->
```js
const profile = {timeline: [
    {year: 2010, lang: "Java"},
    {year: 2011, lang: "C#"  },
    {year: 2012, lang: "C#"  },
    {year: 2013, lang: "Java"},
    {year: 2014, lang: "C#"  },
    {year: 2015, lang: "Java"}
]};
```
```js
const addTimeline2016 = (profile) => {
    profile.timeline.push({
        year: 2016,
        lang: "🍣",
        do: "I'm writing a book about modern web development with C#",
        try: "Microsoft MVP"
    });
}
```
<!-- .element: class="fragment" data-fragment-index="1" -->



# Do you enjoy programming?



### Stack Overflow Developer Survey 2016

![Stack Overflow Developer Survey 2016](img/mostpoplang.jpg)

56,033 coders in 173 countries



## Most Popular 2016

Rank | Language or Tech     | Point
-----|----------------------|-------
1    | JavaScript           | 55.4%
2    | SQL (or SQL Server)  | 49.1%
3    | Java                 | 36.3%
4    | C#                   | 30.9%
5    | PHP                  | 25.9%
6    | Python               | 24.9%
7    | C++                  | 19.4%
8    | AngularJS            | 17.9%
9    | Node.js              | 17.2%
10   | C                    | 15.5%
11   | Ruby                 |  8.9%
12   | Objective-C          |  6.5%


![JabbaScript](img/jabbascript.jpeg)



## Most Loved 2016

Rank | Language or Tech | Point
-----|------------------|-------
1    | Rust             | 79.1%
2    | Swift            | 72.1%
3    | F#               | 70.7%
4    | Scala            | 69.4%
5    | Go               | 68.7%
6    | Clojure          | 66.7%
7    | React            | 66.0%
8    | Haskell          | 64.7%
9    | Python           | 62.6%
10   | C#               | 62.0%
11   | Node.js          | 59.6%



# Java / C# #
# Popular         <!-- .element: class="fragment" data-fragment-index="1" -->
# but NOT Loved?  <!-- .element: class="fragment" data-fragment-index="2" -->



## Cursing
###### subreddits from 2013-08 to 2014-07

![Programming language subreddits and their choice of words. all comments (about 300k) written to submissions (about 40k) in respective programming language subreddits from 2013-08 to 2014-07](img/cursing.png)



### Why do programmers not love
### Object-Oriented Programming <!-- .element: class="fragment" data-fragment-index="1" -->
### and prefer                  <!-- .element: class="fragment" data-fragment-index="2" -->
# Functional Programming?       <!-- .element: class="fragment" data-fragment-index="2" -->



## End of Moore's Law

Architectural change  | Fabrication process | Micro​ archi­tecture      | Release Date
----------------------|---------------------|-------------------------|--------------
Tick                  | 14nm                | Haswell                 | 2014-09-05
Tock                  | 14nm                | Skylake                 | 2015-08-05
~~Tick~~Optimization | ~~10nm~~ 14nm        | Skylake                 | 2016
~~Tock~~Process      | 10nm                 | ~~Ice Lake~~Skylake     | 2017
~~Tick~~Architecture | ~~7nm~~ 10nm         | Ice Lake                | 2018
~~Tock~~Optimization | ~~7nm~~ 10nm         | ~~Tiger Lake~~Ice Lake  | 2019
<!-- .element: class="fragment" data-fragment-index="1" -->



## Amdahl's law

<img alt="Amdahl's law" src="img/multiprocessing.gif" height="400pt"/>



## Ubiquitous Language

### Technical expert
<!-- .element: class="fragment" data-fragment-index="1" -->
```java
List<Integer> list = Arrays.asList(0, 1, 2, -1, 2);

for (Integer x : list) {
    if (x <= 1) System.out.println(x);
    else break;
}
```
<!-- .element: class="fragment" data-fragment-index="1" -->

### Domain expert
<!-- .element: class="fragment" data-fragment-index="2" -->
```java
Arrays.asList(0, 1, 2, -1, 2)
      .stream()
      .takeWhile(x -> x <= 1)
      .forEach(System.out::println);
```
<!-- .element: class="fragment" data-fragment-index="2" -->



## Java 8 Stream
>Classes to support functional-style operations on streams of elements, such as map-reduce transformations on collections.

>-JavaDoc Package java.util.stream



## Stream vs. LINQ
-             | Stream               | LINQ
--------------|----------------------|-----------------
Language      | Java                 | C#, F#, VB, Delphi
Since         | 2014                 | 2007
Developer     | Brian Goetz (Oracle) | Anders Hejlsberg (Microsoft)
Influenced by | ?                    | SQL, Haskell



## Stream vs. LINQ
 Stream               | LINQ
----------------------|-----------------
 n/a (Java 9)         | SkipWhile
 n/a (Java 9)         | TakeWhile
 n/a                  | Zip
 n/a                  | Intersect
 n/a                  | Union
 n/a                  | Except
 n/a                  | GroupJoin
 n/a                  | SequenceEqual
 n/a                  | Last




# Talk is cheap.
# Show me the code. <!-- .element: class="fragment" data-fragment-index="1" -->
## - Linus          <!-- .element: class="fragment" data-fragment-index="1" -->



## My
## Happiness & Cursing
# Java & C# #



## Awesome -> Parallel

### Java
```java
protected long countPrimes(int max) {
    return IntStream
            .range(1, max)
            .parallel()
            .filter(this::isPrime)
            .count();
}
```

### C# #
```csharp
protected int countPrimes(int max)
{
    return Enumerable
            .Range(1, max)
            .AsParallel()
            .Where(x => isPrime(x))
            .Count();
}
```


## Parallel Test OS X

Parallel max=10M | Stream      | LINQ 
-----------------|-------------|---------
OFF              | 5,826ms     | 13,529ms
ON               | 2,526ms     |  5,820ms

<br>
* OS X El Capitan Version 10.11.4
* Intel(R) Core(TM) i7-4650U CPU @ 1.70GHz Haswell
* Java x64 build 1.8.0_77-b03
* C# x64 Mono 1.0.0-rc1-16609


## Parallel Test Windows

Parallel max=10M | Stream      | LINQ 
-----------------|-------------|---------
OFF              | 4,610ms     | 5,717ms
ON               |   924ms     | 1,057ms

<br>
* Windows 10 Pro
* Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz Skylake
* Java x64 build 1.8.0_77-b03
* C# x64 clr 1.0.0-rc2-20221



## Hate -> Stream
<!-- .slide: data-background="rgb(0, 0, 0)" -->

### Java
```java
Stream<String> stream = // 👎 var intermediate operations
        Arrays.asList("React", "AngularJS 2") // 👍
              .stream() // 👎 .stream()?
              .distinct();
```

### C# #
```csharp
var enumerable =　// 👍 var
        new List<string>(){"React", "AngularJS 2"}; // 👎 new
```




## Hate -> filter
<!-- .slide: data-background="rgb(0, 0, 0)" -->

### Java
```java
Stream<String> filter = 
        Arrays.asList("React", "AngularJS 2")
              .stream()
              .filter(x -> x.equals("AngularJS 2")); // 👎 filter?
```

### C# #
```csharp
var where = 
        new List<string>(){"React", "AngularJS 2"}
              .Where(x => x == "AngularJS 2"); // <- 👍 we know SQL
```


## Software Metaphors

_                          | Metaphors     | Year
---------------------------|---------------|------
Donald Knuth               | art           | 1998
P.J. Plauger and Kent Beck | driving a car | 1993, 2000
Eric Raymond               | bazaar        | 2000
Andy Hunt and Dave Thomas  | gardening     | 1999
Fred Brooks                | farming, hunting werewolves, or drowning with dinosaurs in a tar pit | 1995



## Hate -> collect
<!-- .slide: data-background="rgb(0, 0, 0)" -->

### Java
```java
List<String> list =
        Arrays.asList("React", "AngularJS 2")
              .stream()
              .filter(x -> x.equals("AngularJS 2"))
              .collect(Collectors.toList()); // 👎 Collectors? Collector?
```

### C# #
```csharp
List<string> list = 
        new List<string>(){"React", "AngularJS 2"}
              .Where(x => x == "AngularJS 2")
              .ToList(); // 👍 short
```



## Hate -> sort
<!-- .slide: data-background="rgb(0, 0, 0)" -->

### Java
```java
Stream<String> reverse =
        Arrays.asList("AngularJS 2", "React")
              .stream()
              .sorted(Comparator.reverseOrder()); // 👎 too long
```

### C# #
```csharp
var reverse = 
        new List<string>(){"AngularJS 2", "React"}
              .OrderByDescending(x => x); // 👎 lambda
```



## Hate -> sum
<!-- .slide: data-background="rgb(0, 0, 0)" -->

### Java
```java
final int sum =
        Arrays.asList(1, 2, 3, 4, 5)
              .stream()
              .mapToInt(x -> x) // 👎 lambda
              .sum();
// or
final long sum =
        Arrays.asList(1, 2, 3, 4, 5)
              .stream()
              .collect(Collectors.summarizingInt(v -> v)) // 👎 lambda
              .getSum();
```

### C# #
```csharp
int sum =
        new List<int>(){1, 2, 3, 4, 5}
              .Sum(); // 👍
```




## Hate -> Type Erasure
<!-- .slide: data-background="rgb(0, 0, 0)" -->

### Java
```java
    int  sum(Function<List<E>, Integer> selector);
    long sum(Function<List<E>, Long>    selector);
```

```bash
Error: java: name clash: // 👎 type erasure
sum(java.util.function.Function<java.util.List<E>,java.lang.Long>) and
sum(java.util.function.Function<java.util.List<E>,java.lang.Integer>)
have the same erasure
```

### C# #
```csharp
    int  sum(Func<List<E>, int>  selector);
    long sum(Func<List<E>, long> selector); // 👍
```



![Coding Horror](img/coding-horror.png)



## [Scala Fukuoka 2016](http://scala.connpass.com/event/26674/)
<!-- .slide: data-background="rgb(255, 255, 255)" -->
(![Scala Fukuoka](img/scala_fukuoka.png))[http://scala.connpass.com/event/26674/]



# Thanks!



## References

1. Wikipedia, Java version history, https://en.wikipedia.org/wiki/Java_version_history
1. Stack Overflow, Stack Overflow Developer Survey Results 2016, https://stackoverflow.com/research/developer-survey-2016
1. GitHub, Programming language subreddits and their choice of words, https://github.com/Dobiasd/programming-language-subreddits-and-their-choice-of-words
1. Wikipedia, Tick-Tock model, https://en.wikipedia.org/wiki/Tick-Tock_model
1. Steve McConnell, Code Complete, Second Edition
