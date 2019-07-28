## Gentle guide to understand imperative, declarative, functional, reactive programming? 

​	Imperative programming, declarative programming, functional programming, reactive programming – what is these styles, we are coding Java, SQL, JavaScript, and Rx every day! 

![All right, folks, calm down from Django unchained](https://media.giphy.com/media/TLh5VN8Sc6jgtcEPCl/giphy.gif)

If you have heard about these programming styles and you want to know more about the main idea of each one and should you adopt one or a combination of styles? Then this article is made for you.

For most readers here and for years, you probably have used many patterns and techniques from different frameworks and libraries (producer/consumer, listeners, callbacks, observer, queues, topics and events, promises, and futures, data structures,  etc).

> Every 10 years we give new name to what we always do and get excited about it :)

Even if these techniques are pretty old but in certain cases, there are specifications built around them in the last years which makes it more natural to get utilized into any programming language (You can find the full list of all paradigms [here](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type)) More over, as with any design pattern, it becomes easier to explain & discuss.

![Confused](https://media.giphy.com/media/7K3p2z8Hh9QOI/giphy.gif)

First let's start with the fundamental paradigm of all languages, the imperative; 

***The Imperative Programming (IP)*** is the base of any programming language. Developers trained in this programming style are accustomed to describing programs what to do, as well as how to do it. 

Example: Java, C, C++, Python, etc.

Take a look at the code next written in Java and try to find out its purpose:

```java
// Imperative
Map<Integer, List<String>> group(List<String> players) {
    Map<Integer, List<String>> lengthMap = new HashMap<>();
    for (String string : players) {
        List<String> sameLength = null;
        Integer length = string.length();
        if (lengthMap.get(length) == null) {
            sameLength = new ArrayList<>();
            sameLength.add(string);
            lengthMap.put(length, sameLength);
        } else {
            List<String> sameLengthString = lengthMap.get(length);
            sameLengthString.add(string);
        }

    }
    return lengthMap;
}
// Output
{4=[Kean, Mura, Sané], 5=[Salah], 6=[Mahrez, Aguero], 7=[Maguire], 8=[Rashford]}
```

Listing 1



I am certain you read that code very carefully a couple of times. I'll make it easy, the code aims to *group strings by their length*, using the imperative style. There's nothing complicated than the complexity and the number of instructions :D.



​	**Declarative programming (DP)** is the opposite of the imperative style and omits to the computer the "how" by asking what results, to accomplish the task. 

Example: SQL, XSLT, SPARQL, Java 8+, etc.

Let’s see what occurs when we rewrite the `group` method from Listing 1 using the declarative style:

```java
Map<Integer, List<String>> group(List<String> players) {
    return players.stream().collect(Collectors.groupingBy(String::length));
}
// Output
{4=[Kean, Mura, Sané], 5=[Salah], 6=[Mahrez, Aguero], 7=[Maguire], 8=[Rashford]}
```

Listing 2

​	It is simple as "abc". Note first that the code doesn't have any garbage variables in this version and there's no consumed effort on looping over the list.

In the imperative model, you managed the iteration and the program executed precisely as told. In the declarative version, you don't care how the work occurs, as long as it gets done. The implementation of `Collectors.groupingBy()` may vary from any language or platform of execution.



​	**Functional programming (FP)** is based on functions, in the mathematical sense of the word. It deliberately avoids shared mutable state, so it proposes the use of immutable data structures and it features compositionality. This style is supposed declarative except using declarative style doesn't equate to functional style as we talked earlier. This is because functional programming mixes declarative style with higher-order functions.

Example: Scala, Kotlin, JavaScript, Java 8+, etc.

Take a look at the code next written in Java that shows a behavior based on both the imperative and the functional style:

```java
// Functional
void pageVisits() {
    Map<String, Integer> pageVisits = new HashMap<>();

    incrementPageVisitImperative(pageVisits, "google.com");
    incrementPageVisitImperative(pageVisits, "google.com");
    incrementPageVisitImperative(pageVisits, "apple.com");
    
    // incrementPageVisitFunctional(pageVisits, "google.com");
    // incrementPageVisitFunctional(pageVisits, "google.com");
    // incrementPageVisitFunctional(pageVisits, "apple.com");
    
    System.out.println(pageVisits.get("google.com"));
}

void incrementPageVisitImperative(Map<String, Integer> pageVisits, String page) {
    if (!pageVisits.containsKey(page)) {
        pageVisits.put(page, 0);
    }
    pageVisits.put(page, pageVisits.get(page) + 1);
}

void incrementPageVisitFunctional(Map<String, Integer> pageVisits, String page) {
    pageVisits.merge(page, 1, (newCount, oldCount) -> newCount + oldCount);
}

// Output: 
2
```

Listing 3 

​	In Listing 3, the `pageVisits()` method declares a `HashMap` that will hold the number of visits per page. In the meantime, the `incrementPageVisitImperative() or incrementPageVisitFunctional()` methods increment the count for each visit to a given page.

The `incrementPageVisitImperative()` method is written in an imperative style: It checks the page stored in the Map and sets the count at 0 if the page doesn't exist. Then, it increments the number of vistis of the aksed page.

Thinking declaratively requires you to shift the design of this method from "how" to "what": You need to either initialize the count for the given page to 1 or increment the running counter value by one. 

In `incrementPageVisitFunctional()` the page is passed as the first argument to `merge()` which will be the key in the Map. The second argument serves as the initial value for a page. The third argument accepts a lambda expression that returns the sum of its parameters. 

​	Functional programming, and also declarative programming, gained attention after introducing Streams. Anyone has worked with objects, lists, and variables. Well, that's the easy part of an imperative style. Thus, Streams, sequences of data, are supported in many languages like Java, Python, and JavaScript and there's nothing simpler than it, streams could be constructed from anything: variables, inputs, properties, etc. 

With functional and declarative programming, you delegate more notions of basic programming to the compiler: Instead of building a for-loop with an iterator variable and walking through each element, you would say take this list, get me this attribute for a specific element and do this stuff. So that's the biggest benefit of these styles is brevity and kinds of overhead are eliminated from your code.

[Some about Java API and others.]



​	**Reactive Programming (FP)**, a different form of thinking and developing in an event-driven logic, is a replacement for the widely used observer pattern (listeners or callbacks), acts in response to input, and is considered as a flow of data, instead of the traditional flow of control.

> Reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change.
> – Wikipedia

[Rx Java is functional, Some about Rx.]

​	The examples above show are same in form but different in result as imperative style does not react to changes like the second style:

```java
// Imperative
List<Integer> iList = new ArrayList<>();
iList.add(1);
iList.add(2);
for (Integer i : iList) {
    if (i % 2 == 0) {
        double result = i + 0.5;
        System.out.println("Imperative: " + result);
    }
}
iList.add(3);
iList.add(4);
// Output
Imperative: 2.5
```

Listing 4

```java
// Reactive
Subject<Integer> rList = ReplaySubject.create();
rList.onNext(1);
rList.onNext(2);
rList
    .filter(i -> i % 2 == 0)
    .map(i -> i + 0.5)
    .subscribe(result -> System.out.println("Reactive: " + result));
rList.onNext(3);
rList.onNext(4);

// Output
Reactive: 2.5
Reactive: 4.5
```

Listing 5

​	Listing 4 can be written in functional style too. Listing 5 is functional and reactive at the same time. This combination is a specific method of RxJava that enforces the rules of functional programming, especially the property of compositionality.

The code in Listing 4 adds elements to a list, checks the parity of each element and add 0.5 if the last condition if true. Please pay attention wehen adding element to the list after performing the checks are omitted. In Listing 5, the output continued to show results after declaring the behavior and pushing new elements to the list.

​	Embracing more than one style in your projects can help you to learn and do more but you have to admit that none style solves all problems most easily or efficiently but with these styles your code will be short, easier to read and understand. The challenge is to change your ways of reasoning from the imperative style - which is common to a large number of developers - to think declaratively, functionally and reactively. Next steps are to start learning to focus on what you want your programs to do, rather than how you want it done. By allowing the underlying library or platform of each style to manage execution, you will intuitively get to know the building blocks of each style programming.



### Resources

Functional Reactive Programming, [Stephen Blackheath and Anthony Jones]

Functional Programming for Java Developers: Tools for Better Concurrency, Abstraction, and Agility [Dean Wampler]

Reactive Java Programming [Andrea Maglie]

https://developer.ibm.com/articles/j-java8idioms1/

https://ajayiyengar.com/2018/05/05/java-8-imperative-vs-declarative-style-of-coding/

https://codepen.io/HunorMarton/post/imperative-vs-reactive

https://www.scnsoft.com/blog/java-reactive-programming

https://stackoverflow.com/questions/128057/what-are-the-benefits-of-functional-programming

https://www.welcometothejungle.co/en/articles/functional-reactive-programming-architecture