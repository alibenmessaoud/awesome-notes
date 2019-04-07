## Rx Java

RxJava – Reactive Extensions for the JVM – a library for composing asynchronous and event-based programs using observable sequences for the Java VM.

##### Functional Reactive Concepts

Functional programming is the process of building software by composing pure functions, avoiding shared state, mutable data, and side-effects.

Reactive programming is an asynchronous programming paradigm concerned with data streams and the propagation of change.

##### Concepts

There are two key types to understand when working with Rx:

- Observable represents any object that can get data from a data source and whose state may be of interest in a way that other objects may register an interest
- An observer is any object that wishes to be notified when the state of another object changes

##### Start

```java
Iterator<String> iterator = List.of("it-01", "it-02", "it-03").iterator();
// While there is a next element, pull it from the source and print it.
while (iterator.hasNext()) {
	System.out.println("next: " + iterator.next());
}
```

```java
// Subscribe to the Observable. It will push its values to the Subscriber, and it will be printed.
Observable.fromArray("ob-01", "ob-02", "ob-03")
       .subscribe(
              onNext -> System.out.printf("next: %s \n", onNext),
              onError -> System.out.printf("error: %s \n", onError),
              () -> System.out.println("completed")
       );
```

```java
Observable.interval(1, TimeUnit.SECONDS).subscribe(s -> System.out.println(s));
/* Hold main thread for 5 seconds so Observable above has chance to fire */
Thread.sleep(5000);
```

##### Observables Subscriber

Here is what we will cover in this part:
* The Observable
* The Observer
* Other Observable factories
* Single, Completable, and Maybe
* Disposable

Thinking Reactively, the Observable is a push-based, composable iterator.
For a given Observable<T>, it pushes items (emissions) through a series of operators until it finally arrives at a final Observer (the consumer).
At the highest level, an Observable works by passing three types of events:

- onNext(): called onCompleted in v1;
- onComplete(): tells no test onNext() calls
- onError(): intercept the error by the observer using the retry() operator

```java
Observable<String> source = Observable.create( // Observable.fromEmitter in v1;
        emitter -> {
            emitter.onNext("Alpha");
            emitter.onNext("Beta");
            emitter.onComplete();
        }
);
source.subscribe(s -> System.out.println("RECEIVED: " + s));
```

```java
// we can catch errors that may occur within our Observable.create() block 
// and emit them through onError().
// error can be handled by the Observer;
Observable<String> source = Observable.create(emitter -> {
    try {
        emitter.onNext("Gamma");
        emitter.onNext("Delta");
        emitter.onNext("Epsilon");
        emitter.onComplete();
    } catch (Throwable e) {
        emitter.onError(e);
    }
});
source.subscribe(s -> System.out.println("RECEIVED: " + s), Throwable::printStackTrace);
```

```java
// derive new Observables with the map() and filter() operators
// With the map() and filter() operators between the source Observable and Observer, onNext() will hand
// each item to the  map() operator.
// It acts as an intermediary Observer
Observable<String> source = Observable.create(emitter -> {
    try {
        emitter.onNext("Lol");
        emitter.onNext("Epsilon");
        emitter.onComplete();
    } catch (Throwable e) {
        emitter.onError(e);
    }
});
Observable<Integer> lengths = source.map(String::length);
Observable<Integer> filtered = lengths.filter(i -> i >= 5);
filtered.subscribe(s -> System.out.println("RECEIVED: " + s));
```

```java
// will not need to use Observable.create() often
// helpful in hooking into certain sources that are not reactive
// It will invoke the onNext() call for each one and then invoke onComplete() when 
// they all have been pushed:
Observable<String> source1 = Observable.just("Lol", "Beta", "Gamma", "Delta", "Epsilon");
source1.map(String::length).filter(i -> i >= 5).subscribe(s -> System.out.println("RECEIVED: " + s));
// Same for a list
Observable<String> source2 = Observable.fromIterable(List.of("Alpha", "Beta"));
source2.map(String::length).filter(i -> i <= 5).subscribe(s -> System.out.println("RECEIVED: " + s));
```

```java
/*interface Observer<T> {
    void onSubscribe(Disposable d);
    void onNext(T value);
    void onError(Throwable e);
    void onComplete();
}*/
// a source Observable is where your Observable chain starts and where emissions originate.
// It does not know whether the next Observer is another operator or the final Observer 
// at the end of the chain.

Observable<String> source = Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");

Observer<Integer> myObserver = new Observer<>() {
    @Override
    public void onSubscribe(Disposable d) {
        //do nothing with Disposable, disregard for now
    }

    @Override
    public void onNext(Integer value) {
        System.out.println("RECEIVED: " + value);
    }

    @Override
    public void onError(Throwable e) {
        e.printStackTrace();
    }

    @Override
    public void onComplete() {
        System.out.println("Done!");
    }
};
source.map(String::length).filter(i -> i >= 5).subscribe(myObserver);
// is equal to this
source.map(String::length).filter(i -> i >= 5).subscribe(
        i -> System.out.println("RECEIVED: " + i),
        Throwable::printStackTrace,
        () -> System.out.println("Done!")
);

/**
 * It is critical to note that most of the subscribe() overload variants (including the shorthand 
 * lambda ones we just covered) return a Disposable that we did not do anything with. disposables 
 * allow us to disconnect an Observable from an Observer so emissions are terminated early, which
 * is critical for infinite or long-running Observables. We will cover disposables at the end of 
 * this chapter.
 */
/**
 * Can use retry() operators to attempt recovery and resubscribe to an Observable if an error occurs.
 */
```

##### Cold/Hot Observable

```java
/**
 * Cold Observable works much like a music CD. Play it.
 * Cold Observables will replay the emissions to each Observer, ensuring that all Observers 
 * get all the data.
 * Observable.just() and Observable.fromIterable() factories are cold.
 * => Observable sources that emit finite datasets are usually cold
 */
Observable<String> source = Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
source.subscribe(s -> System.out.println("Observer 1 Received:  " + s));
source.subscribe(s -> System.out.println("Observer 2 Received: " + s));

/**
 * Using operators such as map() and filter() against a cold Observable will still
 * maintain the cold nature of the yielded Observables.
 */
```

```java
/**
 * Dave Moten's RxJava-JDBC allows you to create cold Observables built off of SQL database queries.
 * (https://github.com/davidmoten/rxjava-jdbc)
 */
/**
 * ➜  rxjava-tutorial sqlite3 rexon_metals.db
 * SQLite version 3.26.0 2018-12-01 12:34:55
 * Enter ".help" for usage hints.
 * sqlite> CREATE TABLE contacts (
 *    ...>  contact_id INTEGER PRIMARY KEY,
 *    ...>  first_name TEXT NOT NULL,
 *    ...>  last_name TEXT NOT NULL,
 *    ...>  email text NOT NULL UNIQUE,
 *    ...>  phone text NOT NULL UNIQUE
 *    ...> );
 * sqlite> insert into contacts (first_name, last_name, email, phone) values ("a", "bm", "@", "911");
 * sqlite> select * from contacts;
 * 1|ali|ben messaoud|mail|911
 */
Connection conn = new ConnectionProviderFromUrl("jdbc:sqlite:/path/rexon_metals.db").get();
Database db = Database.from(conn);
rx.Observable<String> customerNames = db.select("SELECT FIRST_NAME FROM CONTACTS").getAs(String.class);
customerNames.subscribe(s -> System.out.println(s));

/**
 * This SQL-driven Observable is cold.
 * RxJava-JDBC will run the query each time for each Observer.
 */
```

##### More Factories

```java
/**
 * Note that there is also a long equivalent called Observable.rangeLong() if you need to emit larger numbers.
 */
Observable.range(1, 10).subscribe(s -> System.out.println("RECEIVED: " + s));

/**
 * Automatic one appearance;
 */
/**
 * RECEIVED: 1
 * RECEIVED: 2
 * RECEIVED: 3
 * RECEIVED: 4
 * RECEIVED: 5
 * RECEIVED: 6
 * RECEIVED: 7
 * RECEIVED: 8
 * RECEIVED: 9
 * RECEIVED: 10
 */
```

```java
/**
 * Observables have a concept of emissions over time.
 * Emissions are handed from the source up to the Observer sequentially.
 * But these emissions can be spaced out over time depending on when the source provides them.
 */

/**
 * Observable.interval() will emit infinitely at the specified interval (which is 1 second in this case).
 */
Observable.interval(1, TimeUnit.SECONDS).subscribe(s -> System.out.println(s + " Word "));
Thread.sleep(3000);

/**
 * Shows with a delay of 1s
 */
/**
 * 0 Word
 * 1 Word
 * 2 Word
 */
```

```java
/**
 * does Observable.interval() return a hot or a cold Observable?
 */
Observable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS);
seconds.subscribe(l -> System.out.println("Observer 1: " + l));
Thread.sleep(3000);
seconds.subscribe(l -> System.out.println("Observer 2: " + l));
Thread.sleep(3000);

/**
 * Observer 1: 0 <- starts from 0
 * Observer 1: 1
 * Observer 1: 2
 * Observer 1: 3
 * Observer 2: 0 <- starts from 0 so cold
 * Observer 1: 4 <- continues counting
 * Observer 2: 1
 * Observer 1: 5
 * Observer 2: 2
 */

/**
 * These two observers are actually getting their own emissions, each starting at 0. So this
 * Observable is actually cold.
 */
```

```java
ConnectableObservable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS).publish();
seconds.subscribe(l -> System.out.println("Observer 1: " + l));
seconds.connect();
Thread.sleep(3000);
seconds.subscribe(l -> System.out.println("Observer 2: " + l));
Thread.sleep(3000);

/**
 * Observer 1: 0
 * Observer 1: 1
 * Observer 1: 2
 * Observer 1: 3
 * Observer 2: 3
 * Observer 1: 4
 * Observer 2: 4
 * Observer 1: 5
 * Observer 2: 5
 */

// ConnectableObservable makes cold hot;
```

```java
/**
 * if you have existing libraries that yield Futures, you can easily turn them into Observables
 * via Observable.future():
 */
Future<String> futureValue = Executors.newSingleThreadExecutor()
        .submit(
                () -> {
                    Thread.sleep(1000);
                    return "ali";
                }
        );
Observable.fromFuture(futureValue).map(String::length).subscribe(System.out::println);

/**
 * 3
 */
```

```java
/**
 * Emits nothing and calls onComplete():
 */
Observable<String> empty = Observable.empty();
empty.subscribe(
    System.out::println, 
    error -> System.out.println(error), 
    () -> System.out.println("Done!")
);

/**
 * Done!
 */
```

```java
/**
 * A close cousin of Observable.empty() is Observable.never().
 * The only difference between them is that it never calls onComplete(),
 * forever leaving observers waiting for emissions but never actually giving any
 */
Observable<String> empty = Observable.never();

empty.subscribe(
    System.out::println, 
    e -> System.out.println(e), 
    () -> System.out.println("Done!")
);

/**
 * This Observable is primarily used for testing and not that often in production.
 */
```

```java
Observable.error(new Exception("Crash and burn!"))
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                System.out::println,
                () -> System.out.println("Done!")
        );

/**
 * java.lang.Exception: Crash and burn!
 * at E01Runnable.lambda$main$0(E01Runnable.java:7)
 * at io.reactivex.internal.operators.observable.
 *   ObservableError.subscribeActual(ObservableError.java:32)
 * at io.reactivex.Observable.subscribe(Observable.java:10514)
 * at io.reactivex.Observable.subscribe(Observable.java:10500)
 */

/**
 * You can also provide the exception through a lambda so that it is created
 * from scratch and separate exception instances are provided to each Observer:
 */
Observable.error(() -> new Exception("Crash and burn!"));
```

```java
/**
 * Observable.defer() is a powerful factory due to its ability to create
 * a separate state for each Observer.
 * When using certain Observable factories, you may run into some nuances 
 * if your source is stateful and you want to create a separate state for 
 * each Observer. Your source Observable may not capture something
 * that has changed about its parameters and send emissions that are obsolete. 
 * Here is a simple example: we have an Observable.range() built off two static
 * int properties, start and count.
 */

Observable<Integer> source = Observable.range(start, count);

source.subscribe(i -> System.out.println("Observer 1: " + i));
count = 5;
source.subscribe(i -> System.out.println("Observer 2: " + i));

/**
 * Observer 1: 1
 * Observer 1: 2
 * Observer 1: 3
 * Observer 2: 1
 * Observer 2: 2
 * Observer 2: 3
 */

Observable<Integer> sourceWithDefer = Observable.defer(() -> Observable.range(start, count));
sourceWithDefer.subscribe(i -> System.out.println("Observer 1: " + i));
//modify count
count = 5;
sourceWithDefer.subscribe(i -> System.out.println("Observer 2: " + i));

/**
 * Observer 1: 1
 * Observer 1: 2
 * Observer 1: 3
 * Observer 2: 1
 * Observer 2: 2
 * Observer 2: 3
 * Observer 2: 4
 * Observer 2: 5
 */
```

```java
Observable.just(1 / 0).subscribe(
        i -> System.out.println("RECEIVED: " + i),
        e -> System.out.println("Error Captured: " + e)
);
/**
 * Exception in thread "main" java.lang.ArithmeticException: / by zero
 *     at E01Runnable.main(E01Runnable.java:6)
 *     at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
 */

Observable.fromCallable(() -> 1 / 0).subscribe(
        i -> System.out.println("Received: " + i),
        e -> System.out.println("Error Captured: " + e)
);
/**
 * Error Captured: java.lang.ArithmeticException: / by zero
 */
```

##### Single, Maybe, and Completable.

There are a few specialized flavors of Observable that are explicitly set up for one or no emissions:
-> Single, Maybe, and Completable.

```java
/**
 * Single<T> is essentially an Observable<T> that will only emit one item.
 * It works just like an Observable, but it is limited only to operators that make sense 
 * for a single emission.
 * It has its own SingleObserver interface as well:
 *
 * interface SingleObserver<T> {
 *     void onSubscribe(Disposable d);
 *     void onSuccess(T value);
 *     void onError(Throwable error);
 * }
 */

Single.just("Hello").map(String::length).subscribe(System.out::println, Throwable::printStackTrace);
/**
 * Hello
 */
Observable<String> source = Observable.just("Alpha","Beta","Gamma");
Single<String> nil = source.first("Nothing");
nil.subscribe(System.out::println, System.out::println);
/**
 * Alpha
 */

/**
 * The Single must have one emission, and you should prefer it if you only have one emission to provide;
 * instead of using Observable.just("Alpha"), you should try to use Single.just("Alpha") instead
 */

/**
 * There are operators on Single that will allow you to turn it into an Observable when needed,
 * such as toObservable().
 */

/**
 * If there are 0 or 1 emissions, you will want to use Maybe.
 */
```

```java
/**
 * Maybe is just like a Single except that it allows no emission to occur at all (hence Maybe).
 * MaybeObserver is much like a standard Observer, but onNext() is called onSuccess() instead:
 * public interface MaybeObserver<T> {
 *    void onSubscribe(Disposable d);
 *    void onSuccess(T value);
 *    void onError(Throwable e);
 *    void onComplete();
 * }
 */

/**
 * A given Maybe<T> will only emit 0 or  1 emissions.
 * Maybe.empty() will create a Maybe that yields no emission:
 */

// has emission
Maybe.just(100).subscribe(
    s -> System.out.println("Process 1 received: " + s), 
    Throwable::printStackTrace, 
    () -> System.out.println("Process 1 done!")
);

//no emission
Maybe.empty().subscribe(
    s -> System.out.println("Process 2 received: " + s), 
    Throwable::printStackTrace, 
    () -> System.out.println("Process 2 done!")
);

/**
 * Process 1 received: 100
 * Process 2 done!
 */
```

```java
/**
 * the firstElement() operator, which is similar to first(), but it returns an empty 
 * result if no elements are emitted:
 */

Maybe<String> stringMaybe = Observable.just("Alpha","Beta","Gamma","Delta","Epsilon").firstElement();
stringMaybe.subscribe(
        s -> System.out.println("RECEIVED " + s),
        Throwable::printStackTrace,
        () -> System.out.println("Done!")
);

/**
 * RECEIVED Alpha
 */
```

```java
/**
 * Completable is simply concerned with an action being executed, but it does not receive any emissions.
 * Logically, it does not have onNext() or onSuccess() to receive emissions, 
 * but it does have onError() and onComplete():
 *
 * interface CompletableObserver<T> {
 *     void onSubscribe(Disposable d);
 *     void onComplete();
 *     void onError(Throwable error);
 * }
 */

/**
 * Completable is something you likely will not use often.
 */

Completable.fromRunnable(() -> { /* do something */ }).subscribe(() -> System.out.println("Done!"));

/**
 * Done!
 */
```

##### Disposing

When you subscribe() to an Observable to receive emissions, a stream is created to process these emissions through the Observable chain.
=> this uses resources.
=> dispose of these resources so that they can be garbage-collected

As a matter of fact, you cannot trust the garbage collector to take care of active and no needed subscriptions

```java
public interface Disposable {
 void dispose();
 boolean isDisposed();
}
```

```java
Observable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS);

Disposable disposable = seconds.subscribe(l -> System.out.println("Received: " + l));

sleep(3000);

//dispose and stop emissions
disposable.dispose();

//sleep 3 seconds to prove
//there are no test emissions
sleep(3000);
```

```java
Observable<Long> source = Observable.interval(1, TimeUnit.SECONDS);

Observer<Integer> myObserver = new Observer<Integer>() {
    private Disposable disposable;

    @Override
    public void onSubscribe(Disposable disposable) {
        this.disposable = disposable;
    }

    @Override
    public void onNext(Integer value) {
        //has access to Disposable
    }

    @Override
    public void onError(Throwable e) {
        //has access to Disposable
    }

    @Override
    public void onComplete() {
        //has access to Disposable
    }
};

/**
 * Disposable disposable = source.subscribeWith(myObserver);
 */
```

```java
private static final CompositeDisposable disposables = new CompositeDisposable();
```

```java
Observable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS);

Disposable disposable1 = seconds.subscribe(l -> System.out.println("Observer 1: " + l));
Disposable disposable2 = seconds.subscribe(l -> System.out.println("Observer 2: " + l));

//put both disposables into CompositeDisposable
disposables.addAll(disposable1, disposable2);

sleep(2000);
//dispose all disposables
disposables.dispose();

//check there are no test emissions
sleep(1000);
```

```java
Observable<Integer> source = Observable.create(observableEmitter -> {
    try {
        for (int i = 0; i < 1000; i++) {
            while (!observableEmitter.isDisposed()) {
                observableEmitter.onNext(i);
            }
            if (observableEmitter.isDisposed())
                return;
        }
        observableEmitter.onComplete();
    } catch (Throwable e) {
        observableEmitter.onError(e);
    }
});
source.subscribe(System.out::println);
```

##### Basic operations

```java
/**
 * The  filter() operator accepts Predicate<T> for a given Observable<T>
 * This means that you provide it a lambda that qualifies each emission by mapping it to a Boolean value,
 * and emissions with false will not go forward.
 */
Observable
        .just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .filter(s -> s.length() != 5)
        .subscribe(s -> System.out.println("RECEIVED: " + s));
```

```java
/**
 * The take() operator has two overloads.
 * One will take a specified number of emissions and then call  onComplete() after it captures all of them.
 * It will also dispose of the entire subscription so that no test emissions will occur.
 */
Observable
        .just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .take(3)
        .subscribe(s -> System.out.println("RECEIVED: " + s));

/**
 * Note that if you receive fewer emissions than you specify in your take() function, it will simply
 * emit what it does get and then call the onComplete() function.
 */
Observable.just("Alpha").take(3).subscribe(s -> System.out.println("RECEIVED: " + s));
```

```java
/**
 * The other overload will take emissions within a specific time duration and then call  onComplete().
 * Just keep in mind that it will internally queue emissions until its onComplete() function is called,
 * and then it can logically identify and emit the last emissions.
 */
Observable
        .interval(300, TimeUnit.MILLISECONDS)
        .take(2, TimeUnit.SECONDS)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

sleep(5000);

/**
 * RECEIVED: 0
 * RECEIVED: 1
 * RECEIVED: 2
 * RECEIVED: 3
 * RECEIVED: 4
 * RECEIVED: 5
 */

System.out.println("----------------");

Observable.just(100, 200, 300, 400).takeLast(2).subscribe(i -> System.out.println("RECEIVED: " + i));
```

```java
/**
 * The skip() operator does the opposite of the take() operator.
 * It will ignore the specified number of emissions and then emit the ones that follow.
 */

Observable.range(1,100).skip(90).subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * Just like the take() operator, there is also an overload accepting a time duration.
 * There is also a skipLast() operator, which will skip the last specified number of items
 * (or time duration) before the onComplete() event is called.
 * Just keep in mind that the skipLast() operator will queue and delay emissions until it confirms
 * the last emissions in that scope.
 */
```

```java
Observable
        .range(1,100)
        .takeWhile(i -> i <= 15)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

Observable
        .range(1,100)
        .skipWhile(i -> i <= 95)
        .subscribe(i -> System.out.println("RECEIVED: " + i));
```

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .map(String::length)
        .distinct()
        .subscribe(i -> System.out.println("RECEIVED: " + i));
/**
 * RECEIVED: 5
 * RECEIVED: 4
 * RECEIVED: 7
 */

Observable
        .just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .distinct(String::length)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * RECEIVED: Alpha
 * RECEIVED: Beta
 * RECEIVED: Epsilon
 */
```

```java
Observable
        .just(1, 1, 1, 2, 2, 3, 3, 2, 1, 1)
        .distinctUntilChanged()
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * RECEIVED: 1
 * RECEIVED: 2
 * RECEIVED: 3
 * RECEIVED: 2
 * RECEIVED: 1
 */

Observable
        .just("Alpha", "Beta", "Zeta", "Eta", "Gamma", "Delta")
        .distinctUntilChanged(String::length)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * RECEIVED: Alpha
 * RECEIVED: Beta
 * RECEIVED: Eta
 * RECEIVED: Gamma
 */
```

```java
Observable
        .just("Alpha", "Beta", "Zeta", "Eta", "Gamma", "Delta")
        .elementAt(3)
        .subscribe(i -> System.out.println("RECEIVED: " + i));
```

```java
Observable
        .just("1/3/2016", "5/9/2016", "10/12/2016")
        .map(s -> LocalDate.parse(s, DateTimeFormatter.ofPattern("M/d/yyyy")))
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * RECEIVED: 2016-01-03
 * RECEIVED: 2016-05-09
 * RECEIVED: 2016-10-12
 */
```

```java
Observable<Object> one = Observable.just("Alpha", "Beta", "Gamma").map(s -> (Object) s);
Observable<Object> two = Observable.just("Alpha", "Beta", "Gamma").cast(Object.class);
```

```java
Observable
        .just("Coffee", "Tea", "Espresso", "Latte")
        .startWith("MENU:")
        .subscribe(System.out::println);

Observable
        .just("Coffee", "Tea", "Espresso", "Latte")
        .startWithArray("MENU","----------------")
        .subscribe(System.out::println);
```

```java
Observable.just("Alpha","Beta","Gamma","Delta","Epsilon")
        .filter(s -> s.startsWith("Z"))
        .defaultIfEmpty("None")
        .subscribe(System.out::println);
/**
 * None
 */
```

```java
Observable
        .just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .filter(s -> s.startsWith("Z"))
        .switchIfEmpty(Observable.just("Zeta", "Eta", "Theta"))
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );
/**
 * RECEIVED: Zeta
 * RECEIVED: Eta
 * RECEIVED: Theta
 */
```

```java
Observable
        .just(6, 2, 5, 7, 1, 4, 9, 8, 3)
        .sorted().subscribe(System.out::println);

Observable
        .just(6, 2, 5, 7, 1, 4, 9, 8, 3)
        .sorted(Comparator.reverseOrder())
        .subscribe(System.out::println);

Observable
        .just("Alpha", "Beta", "Gamma" ,"Delta", "Epsilon")
        .sorted((x,y) -> Integer.compare(x.length(), y.length())) //sorted(Comparator.comparingInt(String::length))
        .subscribe(System.out::println);

Observable
        .just("Alpha", "Beta", "Gamma" ,"Delta", "Epsilon")
        .sorted(Comparator.comparingInt(String::length))
        .subscribe(System.out::println);
```

```java
/**
 * We can postpone emissions using the delay() operator.
 */
Observable
        .just("Alpha", "Beta", "Gamma" ,"Delta", "Epsilon")
        .delay(3, TimeUnit.SECONDS)
        .subscribe(s -> System.out.println("Received: " + s));
sleep(5000);
```

```java
Observable
        .just("Alpha", "Beta", "Gamma" ,"Delta", "Epsilon")
        .repeat(2)
        .subscribe(s -> System.out.println("Received: " + s));
```

```java
/**
 * The scan() method is a rolling aggregator.
 */

Observable.just(5, 3, 7, 10, 2, 14)
        .scan((accumulator, next) -> accumulator + next)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: 5
 * Received: 8
 * Received: 15
 * Received: 25
 * Received: 27
 * Received: 41
 */

Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .scan(0, (total, next) -> total + 1)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: 0
 * Received: 1
 * Received: 2
 * Received: 3
 * Received: 4
 * Received: 5
 */
```

