# Reactive Programming

Reactive, reactive programming, reactive architecture, etc., there are plenty of explanations and definitions out there on the Internet. Wikipedia, Stackoverflow and Reactive Manifesto are not clear. Everybody is talking about events, streams, observables, systems, and architectures, about being or going reactive, how when and why ?! It is not so clear too?

Asynchronous behavior is not a new thing. A click event on a web page using jQuery fires that callback code to network and to render the response asynchronously is a common example of events and callbacks; Streams in Java 8 to work with collections is such easy thing using the "functional" magic kicks but it is not Reactive enough because these streams are pull-based.

So let's go deep and spot the light on the keyword reactive but let me at first define some basic concepts: In general, a stream is a sequence of ongoing signals ordered in time. It can emit three different things: a value, an error, or a completed signal. 

There's nothing simpler than a stream, it could and should be constructed from anything: variables, user inputs, properties, caches, data structures, etc. Reactive involves the production and consumption of streams of data as they occur but under the umbrella of a reactive system. Unlike streams in Java, data flows in a reactive world undergo by new behaviors and the change they made can be used in the next steps of that pipeline. This is translated by the concept of propagation of change. The changes generated in the application can be analyzed and propagated to provide fast and consistent response times (responsive). When something occurs in any step of the process, the stream is so "intelligent" to overcome error situations (resilient). Also, due to an increasing workload, the stream can scale to keep the same quality of service (scalable). 

So let's sum up: Reactive programming focuses on the asynchronous management of *xfinite* data flow of events and dealing with them by reacting related to a change in an asynchronous way, delegating the process to be handled and without blocking. 

So, in the reactive programming jargon, the stream is called Observable. Observable means Observer, the Schedulers, etc in that world. We can see these friends in many applications based on the Reactive Extensions RxJava, RxJS, Reactor, Akka Streams, Vert.x and Ratpack. 

An Observable is nothing but the data flow. It pushes data as they occur periodically or only once in its life cycle. The Observer is the one who will consume that data. The Observer can perform various operations (transform, extract, wait, merge, zip, collect, etc.) based on the type of the emitted signal or data. Schedulers are the component that tells Observables and Observers, on which thread they should run.

Here’s a short real-life example. Say, it’s Sunday and John wants to watch with his best friend Mary a movie, eating pizza and that after finishing cleaning first. Let’s outline the options he has:

- John finishes his cleaning tasks. Then goes and orders the pizza, waits till it’s done. Then he picks up his friend. And finally, they go and watch the movie: The sync approach.
- John finishes his cleaning tasks. Then, he orders his pizza by phone, phones Mary, and goes to pick up her. Meanwhile, he missed Pizza delivery due to a traffic jam. He starts watching the movie with Mary and waiting for pizza-man to show up: The async approach. 
- John starts cleaning, phones Mary to invite her. Before going to pick up her, he orders pizza. By making the round trip to pick up Mary, the pizza is ready. So they head home and turn the movie on. This is what the reactive approach is about. You wait till all async actions (changes) are completed and then proceed with further actions.
  You should notice that reactive programming and reactive systems are not the same things here. Reactive systems represent the next level of ‘reactivity’. 

Next is when to use Reactive Programming ? You will not need it. Don't apply it because the world is doing it! For example, when there is no ‘live’ data or a large number of users who change data simultaneously, keep the code simple imperative. 

However, providing a highly interactive experience often requires you to combine events, have conditional logic, and handle failure scenarios gracefully — this is where reactive programming shines.

So, coding in Reactive Programming and considering data as a continuous stream of events, that can help to level up the abstraction of code so you can focus on the interdependence of events that define the business logic. This abstraction layer provides a very powerful tool for doing async and non-blocking programming: A significant changes to your coding style. 

-----

Reactor 3 is a library built around the Reactive Streams specification, bringing the paradigm of Reactive Programming (RP) on the JVM. 

Reactive Streams specification: Publisher, Subscriber.

To create a Reactive Stream Publisher or Subscriber: Use a lib and TCK.

##### Flux

A `Flux<T>` is a Reactive Streams `Publisher` used to generate, transform, orchestrate Flux sequences. It can emit 0 to *n* `<T>` elements (`onNext` event) then either completes or errors (`onComplete` and `onError`). If no terminal event is triggered, the `Flux` is infinite.

`static <T> Flux<T> empty()` => `Flux.empty()` 

`static <T> Flux<T> just(T... data)` => `Flux.just("foo", "bar");`

`static <T> Flux<T> fromIterable(Iterable<? extends T> it)` => `Flux.fromIterable(Arrays.asList("foo", "bar"));`

`static <T> Flux<T> error(Throwable error)` => `return Flux.error(new IllegalStateException("Error"));`

`static Flux<Long> interval(Duration period)` => `Flux.interval(ofMillis(100));`

##### Mono

A `Mono<T>` is a Reactive Streams `Publisher`  used to generate, transform, orchestrate Mono sequences. It can emit at most 1 element and complete with it then either empty or error. Mono is equivalent of a `Runnable` task completing. 

`static <T> Mono<T> empty()` => `Mono.empty();`

`monoWithNoSignal` => `Mono.never();`

`Mono.just("foo");`

`Mono.error(new IllegalStateException("Error"));`

##### StepVerifier

Test using a `StepVerifier`

`StepVerifier.create(T<Publisher>).{expectations...}.verify()`

```java
// check that the flux parameter emits "foo" and "bar" elements then completes successfully.
void expectFooBarComplete(Flux<String> flux) {
	StepVerifier.create(flux)
        .expectNext("foo", "bar")
        .expectComplete()
        .verify();
}
```

```java
// check that the flux parameter emits "foo" and "bar" elements then a RuntimeException error.
void expectFooBarError(Flux<String> flux) {
    StepVerifier.create(flux)
        .expectNext("foo", "bar")
        .expectErrorMatches(throwable -> throwable instanceof RuntimeException)
        .verify();
}
```

```java
// check that the flux parameter emits a User with "swhite"username
// and another one with "jpinkman" then completes successfully.
void expectSkylerJesseComplete(Flux<User> flux) {
    StepVerifier.create(flux)
        .expectNextMatches(user -> user.getUsername().equals("swhite"))
        .assertNext(user -> assertThat(user.getUsername().equals("jpinkman")))
        .verifyComplete();
}
```

```java
// Expect 10 elements then complete and notice how long the test takes.
void expect10Elements(Flux<Long> flux) {
    StepVerifier.create(flux)
        .expectNextCount(10)
        .verifyComplete();
}
```

```java
// Expect 3600 elements at intervals of 1 second, and verify quicker than 3600s
// by manipulating virtual time thanks to StepVerifier#withVirtualTime, notice how long the test takes
void expect3600Elements(Supplier<Flux<Long>> supplier) {
    StepVerifier.withVirtualTime(supplier)
        .thenAwait(ofSeconds(3600))
        .expectNextCount(3600)
        .verifyComplete();
    
    StepVerifier.withVirtualTime(() -> Mono.delay(Duration.ofHours(3)))
            .expectSubscription()
            .expectNoEvent(Duration.ofHours(2))
            .thenAwait(Duration.ofHours(1))
            .expectNextCount(1)
            .expectComplete()
            .verify();
}
```

##### Transform

```java
// TODO Capitalize the user username, firstname and lastname
Mono<User> capitalizeOne(Mono<User> mono) {
    return mono.map(this::getUserCapitalized);
}

// TODO Capitalize the users username, firstName and lastName
Flux<User> capitalizeMany(Flux<User> flux) {
    return mono.map(this::getUserCapitalized);
}


// TODO Capitalize the users username, firstName and lastName using #asyncCapitalizeUser
Flux<User> asyncCapitalizeMany(Flux<User> flux) {
    return flux.flatMap(this::asyncCapitalizeUser);
}

Mono<User> asyncCapitalizeUser(User u) {
    return Mono.just(new User(u.getUsername().toUpperCase(), 
                              u.getFirstname().toUpperCase(), 
                              u.getLastname().toUpperCase()
                             )
                    );
}

private User getUserCapitalized(User user) {
    return new User(user.getUsername().toUpperCase(),
                    user.getFirstname().toUpperCase(),
                    user.getLastname().toUpperCase()
                   );
}
```

- `map` is for synchronous, non-blocking, 1-to-1 transformations
- `flatMap` is for asynchronous (non-blocking) 1-to-N transformations

The difference is visible in the method signature:

- `map` takes a `Function<T, U>` and returns a `Flux<U>`
- `flatMap` takes a `Function<T, Publisher<V>>` and returns a `Flux<V>`

In RxJava:

- `map` returns an object of type T
- `flatmap` returns an Observable.

##### Merge

The caveat here is that values from `flux1` arrive with a delay, so in the resulting `Flux` we start seeing values from `flux2` first.

```java
// Merge flux1 and flux2 values with interleave
Flux<User> mergeFluxWithInterleave(Flux<User> flux1, Flux<User> flux2) {
	return flux1.mergeWith(flux2);
}
```

Concat will wait for `flux1` to complete before it can subscribe to `flux2`, ensuring that all the values from `flux1` have been emitted, thus preserving an order corresponding to the source.

```java
// Merge flux1 and flux2 values with no interleave (flux1 values and then flux2 values)
Flux<User> mergeFluxWithNoInterleave(Flux<User> flux1, Flux<User> flux2) {
    return flux1.concatWith(flux2);
}
```

```java
// TODO Create a Flux containing the value of mono1 then the value of mono2
Flux<User> createFluxFromMultipleMono(Mono<User> mono1, Mono<User> mono2) {
    return Flux.concat(mono1, mono2);
}
```

##### Request

There is one aspect to it that we didn't cover: the volume control. In Reactive Streams terms this is called **back pressure**. It is a feedback mechanism that allows a `Subscriber` to signal to its `Publisher` how much data it is prepared to process, limiting the rate at which the `Publisher`produces data.

This control of the demand is done at the `Subscription` level: a `Subscription` is created for each `subscribe()` call and it can be manipulated to either `cancel()` the flow of data or tune demand with `request(long)`.

Making a `request(Long.MAX_VALUE)` means an **unbounded demand**, so the `Publisher` will emit data at its fastest pace.

```java
// Create a StepVerifier that initially requests all values and expect 4 values to be received
StepVerifier requestAllExpectFour(Flux<User> flux) {
    return create(flux)
        .thenRequest(Long.MAX_VALUE)
        .expectNextCount(4)
        .expectComplete();
}


// Create a StepVerifier that initially requests 1 value and expects User.SKYLER then requests another value and expects User.JESSE.
StepVerifier requestOneExpectSkylerThenRequestOneExpectJesse(Flux<User> flux) {
    return create(flux)
        .thenRequest(1L).expectNext(SKYLER)
        .thenRequest(1L).expectNext(JESSE)
        .thenCancel();
}

// Return a Flux with all users stored in the repository that prints automatically logs for all Reactive Streams signals
Flux<User> fluxWithLog() {
    return repository.findAll().log();
}

// Return a Flux with all users stored in the repository that prints "Starring:" on subscribe, "firstname lastname" for all values and "The end!" on complete
Flux<User> fluxWithDoOnPrintln() {
    return repository.findAll()
        .doOnSubscribe(s -> System.out.println("Starring"))
        .doOnNext(u -> System.out.println(u.getFirstname() + " " + u.getLastname()))
        .doOnComplete(() -> System.out.println("The end!"));
}
```

##### Error

```java
// TODO Return a Mono<User> containing User.SAUL when an error occurs in the input Mono, else do not change the input Mono.
Mono<User> betterCallSaulForBogusMono(Mono<User> mono) {
    return mono.onErrorReturn(User.SAUL);
}


// TODO Return a Flux<User> containing User.SAUL and User.JESSE when an error occurs in the input Flux, else do not change the input Flux.
Flux<User> betterCallSaulAndJesseForBogusFlux(Flux<User> flux) {
    return flux.onErrorResume(e -> Flux.just(User.SAUL, User.JESSE));
}

// TODO Implement a method that capitalizes each user of the incoming flux using the
// #capitalizeUser method and emits an error containing a GetOutOfHereException error
Flux<User> capitalizeMany(Flux<User> flux) {
    return flux.map(u -> {
        try {
            return capitalizeUser(u);
        } catch (GetOutOfHereException e) {
            throw Exceptions.propagate(e);
        }
    });
}

User capitalizeUser(User user) throws GetOutOfHereException {
    if (user.equals(User.SAUL)) {
        throw new GetOutOfHereException();
    }
    return new User(user.getUsername(), user.getFirstname(), user.getLastname());
}

protected final class GetOutOfHereException extends Exception {
}
```

```java
// Create a Flux of user from Flux of username, firstname and lastname.
Flux<User> userFluxFromStringFlux(Flux<String> usernameFlux, Flux<String> firstnameFlux, Flux<String> lastnameFlux) {
    return Flux.zip(usernameFlux, firstnameFlux, lastnameFlux)
        .map(t -> new User(t.getT1(), t.getT2(), t.getT3()));
}

// Return the mono which returns its value faster
Mono<User> useFastestMono(Mono<User> mono1, Mono<User> mono2) {
    return Mono.first(mono1, mono2);
}

// Return the flux which returns the first value faster
Flux<User> useFastestFlux(Flux<User> flux1, Flux<User> flux2) {
    return Flux.firstEmitting(flux1, flux2);
}

// Convert the input Flux<User> to a Mono<Void> that represents the complete signal of the flux
Mono<Void> fluxCompletion(Flux<User> flux) {
    return flux.then();
}

// Return a valid Mono of user for null input and non null input user (hint: Reactive Streams do not accept null values)
Mono<User> nullAwareUserToMono(User user) {
    return Mono.justOrEmpty(user);
}

// Return the same mono passed as input parameter, expect that it will emit User.SKYLER when empty
Mono<User> emptyToSkyler(Mono<User> mono) {
    return mono.defaultIfEmpty(User.SKYLER);
}
```

##### Adapt

You can make RxJava2 and Reactor 3 types interact without a single external library.

In the first two examples we will adapt from `Flux` to `Flowable`, which implements `Publisher`, and vice-versa.

The next two examples are a little trickier: we need to adapt between `Flux` and `Observable`, but the later doesn't implement `Publisher`.

```java
// Adapt Flux to RxJava Flowable
Flowable<User> fromFluxToFlowable(Flux<User> flux) {
    return Flowable.fromPublisher(flux);
}

// Adapt RxJava Flowable to Flux
Flux<User> fromFlowableToFlux(Flowable<User> flowable) {
    return Flux.from(flowable);
}
```

```java
// Adapt Flux to RxJava Observable
Observable<User> fromFluxToObservable(Flux<User> flux) {
    return Observable.fromPublisher(flux);
}

// Adapt RxJava Observable to Flux
Flux<User> fromObservableToFlux(Observable<User> observable) {
    return Flux.from(observable.toFlowable(BackpressureStrategy.BUFFER));
}
```

```java
// Adapt Mono to RxJava Single
Single<User> fromMonoToSingle(Mono<User> mono) {
    return Single.fromPublisher(mono);
}

// Adapt RxJava Single to Mono
Mono<User> fromSingleToMono(Single<User> single) {
    return Mono.from(single.toFlowable());
}
```

```java
// Adapt Mono to Java 8+ CompletableFuture
CompletableFuture<User> fromMonoToCompletableFuture(Mono<User> mono) {
    return mono.toFuture();
}

// Adapt Java 8+ CompletableFuture to Mono
Mono<User> fromCompletableFutureToMono(CompletableFuture<User> future) {
    return Mono.fromFuture(future);
}
```

| RxJava 2                                                     | Reactor                                                      | Purpose                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`Completable`](http://reactivex.io/RxJava/javadoc/io/reactivex/Completable.html) | N/A                                                          | Completes successfully or with failure, without emitting any value. Like `CompletableFuture<Void>` |
| [`Maybe`](http://reactivex.io/RxJava/javadoc/io/reactivex/Maybe.html) | [`Mono`](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html) | Completes successfully or with failure, may or may not emit a single value. Like an asynchronous `Optional<T>` |
| [`Single`](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html) | N/A                                                          | Either complete successfully emitting exactly one item or fails. |
| [`Observable`](http://reactivex.io/RxJava/javadoc/io/reactivex/Observable.html) | N/A                                                          | Emits an indefinite number of events (zero to infinite), optionally completes successfully or with failure. Does not support backpressure due to the nature of the source of events it represents. |
| [`Flowable`](http://reactivex.io/RxJava/javadoc/io/reactivex/Flowable.html) | [`Flux`](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html) | Emits an indefinite number of events (zero to infinite), optionally completes successfully or with failure. Support backpressure (the source can be slowed down when the consumer cannot keep up) |

##### Reactive to blocking

```java
// Return the user contained in that Mono
User monoToValue(Mono<User> mono) {
    return mono.block();
}

// Return the users contained in that Flux
Iterable<User> fluxToValues(Flux<User> flux) {
    return flux.toIterable();
}
```

##### Blocking to Reactive

```java
// Create a Flux for reading all users from the blocking repository deferred until the flux is subscribed, and run it with an elastic scheduler
Flux<User> blockingRepositoryToFlux(BlockingRepository<User> repository) {
    return Flux.defer(() -> Flux.fromIterable(repository.findAll()))
        .subscribeOn(Schedulers.elastic());
}


// Insert users contained in the Flux parameter in the blocking repository using an elastic scheduler and return a Mono<Void> that signal the end of the operation
Mono<Void> fluxToBlockingRepository(Flux<User> flux, BlockingRepository<User> repository) {
    return flux
        .publishOn(Schedulers.elastic())
        .doOnNext(repository::save)
        .then();
}
```

