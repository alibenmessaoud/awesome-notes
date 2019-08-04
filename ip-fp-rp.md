## Gentle guide to understand imperative, declarative, functional, reactive programming? 

​	Imperative programming, declarative programming, functional programming, reactive programming – what is these styles. Everyday, we are coding and use several languages and syntaxes like in Java, SQL, JavaScript, Rx, etc. 

If you have heard about these programming styles and you want to know more about the main idea of each one and should you adopt one or a combination of styles in your next project or apply while refactoring an existing project? Then this article is made for you.

For most readers here and for years, you probably have used many patterns and techniques from different frameworks and libraries (producer/consumer, listeners, callbacks, observer, queues, topics and events, promises, and futures, data structures,  etc).

> Every 10 years we give new name to what we always do and get excited about it :)

Even if these techniques are pretty old but in certain cases, there are specifications built around them in the last years which makes it more natural to get utilized into any programming language (You can find the full list of all paradigms [here](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type)) More over, as with any design pattern, it becomes easier to explain & discuss.

![Confused](https://media.giphy.com/media/7K3p2z8Hh9QOI/giphy.gif)

​	First let's start with the fundamental paradigm of all languages, the imperative; 

***The Imperative Programming (IP)*** is the base of any programming language. Developers trained in this programming style are accustomed to describing programs what to do, as well as how to do it. 

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



I am certain you read the code carefully a couple of times. I'll make it easy, the code aims to *group strings by their length*, using the imperative style. The code is complicated and the complexity of this method due to the for loop and the number of declarations penalizes this approach. 

We can find the same as this code in many languages like C, C++, Python, etc. Isn't it going the same way it went with the pointers in C++?

So what's next?

The idea lead to use the functional style of programming along with Object style, a hybrid language with declarative style of programming instead of imperative style. Do I used functional and declarative at the same time? We will clarify these terms in the next lines.



![All right, folks, calm down from Django unchained](https://media.giphy.com/media/TLh5VN8Sc6jgtcEPCl/giphy.gif)



​	**Declarative programming (DP)** is the opposite of the imperative style and omits to the computer the "how" by asking what results, to accomplish the task.

As an example, structuring a page in HTML and formating it with XSLT are used in some existing applications. SQL queries declare what fields to retrieve and the database engine does the job to answer that query. In Java world and since its 8th version, declarative style had been add to empower developers.

Let’s rewrite in Java the `group` method from Listing 1 using the declarative style:

```java
Map<Integer, List<String>> group(List<String> players) {
    return players.stream().collect(Collectors.groupingBy(String::length));
}

// Output
{4=[Kean, Mura, Sané], 5=[Salah], 6=[Mahrez, Aguero], 7=[Maguire], 8=[Rashford]}
```

Listing 2

​	It is simple as "abc". Note first that the code doesn't have any garbage variables in this version and there's no consumed effort on looping over the list.

In the opposite style, we managed the iteration and the program executed line by line as told. In the declarative version, we didn't care about how the work has been done, as long as the method returned the result. Each platform on each the declarative code is built-in and executed does the job and the implementation of `Collectors.groupingBy()` may vary from any language or platform of execution.



​	**Functional programming (FP)** is based on functions, in the mathematical sense of the word. It deliberately avoids shared mutable state, so it proposes the use of immutable data structures and it features compositionality. This style is supposed declarative except using declarative style doesn't equate to functional style as we talked earlier. This is because functional programming mixes declarative style with higher-order functions.

This style is the base of Scala, Kotlin, JavaScript, Java 8+, etc.

Take a look at the code next written in Java that shows a behavior based on both the imperative and the functional style:

```java
// Functional
void playerVotes() {
    Map<String, Integer> playerVotes = new HashMap<>();

    incrementVoteImperative(playerVotes, "Salah");
    incrementVoteImperative(playerVotes, "Salah");
    incrementVoteImperative(playerVotes, "Rashford");
    
    // incrementVoteFuntional(playerVotes, "Salah");
    // incrementVoteFuntional(playerVotes, "Salah");
    // incrementVoteFuntional(playerVotes, "Rashford");
    
    System.out.println(playerVotes.get("Salah"));
}

void incrementVoteImperative(Map<String, Integer> playerVotes, String player) {
    if (!playerVotes.containsKey(player)) {
        playerVotes.put(player, 0);
    }
    playerVotes.put(player, playerVotes.get(player) + 1);
}

void incrementVoteFuntional(Map<String, Integer> playerVotes, String player) {
    playerVotes.merge(player, 1, (newCount, oldCount) -> newCount + oldCount);
}

// Output: 
2
```

Listing 3 

​	In Listing 3, the `playerVotes()` method declares a `HashMap` that will hold the number of votes per player. In the meantime, the `incrementVoteImperative()` or `incrementVoteFuntional()` methods increment the count for each vote to a given player.

The `incrementVoteImperative()` method is written in an imperative style: It checks the player key stored in the Map and sets the count at 0 if the player doesn't exist. Then, it increments the number of votes of the asked player.

Thinking declaratively requires you to shift the design of this method from "how" to "what": You need to either initialize the count for the given player to 1 or increment the running counter value by one. 

In `incrementVoteFuntional()` the player is passed as the first argument to `merge()` which will be the key in the Map. The second argument serves as the initial value for a player vote. The third argument accepts a lambda expression that returns the sum of its parameters. 

​	Functional programming, and also declarative programming, gained attention after introducing lambda expressions and Streams. Anyone has worked with objects, lists, and variables and that's an easy part. Thus, Streams, sequences of data, are adopted in JavaScript and in Java as `java.util.concurrent.Flow` since Java 9. After that, a stream could be constructed from anything: variables, inputs, properties, etc. 

With functional and declarative programming, we could delegate more notions of basic programming to the compiler: Instead of building a for-loop with an iterator variable and walking through each element, we would say take this list, get me this attribute for a specific element and do this logic. So that's the biggest benefit of these styles is brevity and kinds of overhead are eliminated from your code.



​	**Reactive Programming (FP)**, a different form of thinking and developing in an event-driven logic, is a replacement for the widely used observer pattern (listeners or callbacks), acts in response to input, and is considered as a flow of data, instead of the traditional flow of control.

> Reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change.
> – Wikipedia

The reactive programming is not supported natively in Java or any other languages but there are third-party libraries like RxJava, RxJS, Akka and many more which provides these functionality out of the box, easing the adopting and working in a reactive way.

​	The examples below shows the same code in form but a different behavior as the imperative style does not react to changes like the second style:

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

​	Listing 4 can be written in functional style too. Listing 5 is functional and reactive at the same time. This combination is a specific method of Rx Java that enforces the rules of functional programming, especially the property of compositionality.

The code in Listing 4 adds elements to a list, checks the parity of each element and add 0.5 if the last condition if true. Please pay attention when adding element to the list after performing the checks are omitted. In Listing 5, the output continued to show results after declaring the behavior and pushing new elements to the list.

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