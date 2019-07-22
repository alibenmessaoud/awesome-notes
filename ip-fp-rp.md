# Gentle guide to understand imperative, declarative, functional, reactive programming? 

Imperative programming, declarative programming, functional programming, reactive programming – what is this, we are coding Java, SQL, JavaScript, and Rx every day!

![All right, folks, calm down from Django unchained](https://media.giphy.com/media/TLh5VN8Sc6jgtcEPCl/giphy.gif)

If you have seen or heard about all these programming models and you want to know what is the main idea of each one and should you adopt one or a combination of styles? Then this article is made for you.

For most readers here and for years, you probably have used many patterns and techniques from different frameworks and libraries (producer/consumer, listeners, callbacks, observer, queues, topics and events, promises, and futures, data structures,  etc).

> Every 10 years we give new name to what we always do and get excited about it :)

Even if these techniques are pretty old but in certain cases, there are specifications built around them in the last years which makes it more natural to get utilized into any programming language. More over, as with any design pattern, it becomes easier to explain & discuss.

~~If you will start a new project and keywords like reactive and functional programming come in mind, you are in the right place to see which of these you should use for your project.~~

![Confused](https://media.giphy.com/media/7K3p2z8Hh9QOI/giphy.gif)

First let's start with the fundamentals, the definitions: 

**Imperative programming**

Imperative programming is the base of any programming language. Developers trained in this programming style are accustomed to describing programs what to do, as well as how to do it. 

Example: Java, C, C++, Python, etc.

Take a look at the code next and try to find out the purpose of the code.

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



I am certain you read that code very carefully and likely a couple of times. So the code is performing these actions:

1. Create a new hashmap
2. Loop through the list of strings
3. Create a new list
4. If a string has length x and x is already in the map then get the list corresponding to x and add the string to that list, else build a new list and add the string and x its length as the key.
5. Done !

Briefly, the code tries to *group strings by their length*, that is the imperative style.



**Declarative programming**

Declarative programming is the opposite of the imperative style and omits to the computer the "how" by asking what results, to accomplish the task. 

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



What are we doing here:

1. Group all the strings by length
2. Done !

Note first that you don't have any garbage variables in this version. You are also not consuming effort on looping over the list.

In the imperative model, you managed the iteration and the program executed precisely as told. In the declarative version, you don't care how the work occurs, as long as it gets done. The implementation of `Collectors.groupingBy()` may vary.



**Functional programming**

This paradigm is based on functions, in the mathematical sense of the word. It deliberately avoids shared mutable state, so it proposes the use of immutable data structures and it features compositionality. This style is supposed declarative except using declarative style doesn't equate to functional programming as we talked earlier. This is because functional programming mixes declarative style with higher-order functions.

Example: Scala, Kotlin, JavaScript, Java 8+, etc.

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
    pageVisits.merge(page, 1, (newValue, oldValue) -> newValue + oldValue);
}

// Output: 
2
```

Listing 3 

In listing 3, the `pageVisits()` method declares a `HashMap` that will hold the number of page visits per page. In the meantime, the `incrementPageVisitImperative() or incrementPageVisitFunctional()` method increments a count for each visit to the given page.

The `incrementPageVisitImperative()` method is written in an imperative style: It checks the page in the map and sets the count at 0 if the page doesn't exist. Then, it increments the count of that page.

Thinking declaratively requires you to shift the design of this method from "how" to "what": You need to either initialize the count for the given page to 1 or increment the running value by one count. 

In `incrementPageVisitFunctional()` the page is passed as the first argument to `merge()` as a key in the map. The second argument serves as the initial value for a page. The third argument accepts a lambda expression that returns the sum of its parameters. 

Functional programming, and also declarative programming, gained attention after introducing Streams. Anyone has worked with objects, lists, and variables. Well, that's the easy part of an imperative style. Thus, streams, sequences of data, are supported in many languages like Java, Python, and JavaScript and there's nothing simpler than it, streams could be constructed from anything: variables, inputs, properties, etc. 

With functional and declarative programming, you move more basic programming notions to the compiler: Instead of building a for-loop with an iterator variable and walking through each element, you would say take this list, get me this attribute for a specific element and do this stuff. So that's the biggest benefit of these styles is brevity and kinds of overhead are eliminated from your code.

Some about Java API and others.



**Reactive Programming**

Reactive Programming, a different form of thinking and developing in an event-driven logic, is a replacement for the widely used observer pattern (listeners or callbacks), acts in response to input, and is considered as a flow of data, instead of the traditional flow of control.

> Reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change.
> – Wikipedia

The examples above show the "same code" but one in imperative style and the second in reactive style:

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
Subject<Integer> rSubject = ReplaySubject.create();
rSubject.onNext(1);
rSubject.onNext(2);
rSubject
    .filter(i -> i % 2 == 0)
    .map(i -> i + 0.5)
    .subscribe(result -> System.out.println("Reactive: " + result));
rSubject.onNext(3);
rSubject.onNext(4);

// Output
Reactive: 2.5
Reactive: 4.5
```

Listing 5

Listing 5 is functional and reactive at the same time. This combination is a specific method of reactive programming that enforces the rules of functional programming, especially the property of compositionality.

Well the table of programming paradigms is too long and we will not reveal it here but several languages back more than one programming paradigm. The idea is to empower engineers to use the best tools to achieve a goal, admitting that none style solves all problems most easily or efficiently. Java is object-oriented, procedural, imperative, functional,  declarative and supports reactive. Python is functional, object-oriented, imperative, metaprogramming, reflective, scripting, etc. You can find the full list [here](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type).

Some about Rx.

**Conclusion**
Embracing one or several techniques in your projects can help you to learn and do more: your code will be short, easier to read and understand. The challenge is to change your ways of reasoning from the imperative style - which is common to a large number of developers - to think declaratively, functionally and reactively. Next steps are to start learning to focus on what you want your programs to do, rather than how you want it done. By allowing the underlying library or platform of each style to manage execution, you will intuitively get to know the building blocks of each style programming.