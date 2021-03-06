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

##### Reduce operation

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .count()
        .subscribe(s -> System.out.println("Received: " + s));
/**
 * Received: 5
 */
```

```java
Observable.just(5, 3, 7, 10, 2, 14)
        .reduce((total, next) -> total + next)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: 41
 */
```

```java
/**
 * Similar to scan(), there is a seed argument that you can provide that will serve as the initial
 * value to accumulate on.
 */

Observable.just(5, 3, 7, 10, 2, 14)
        .reduce("", (total, next) -> total + (total.equals("") ? "" : ",") +  next)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: 5,3,7,10,2,14
 */
```

```java
Observable.just(5, 3, 7, 11, 2, 14)
        .all(i -> i < 10)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: false
 */

/**
 * When the all() operator encountered 11, it immediately emitted False and called onComplete().
 */
```

```java
Observable.just("2016-01-01", "2016-05-02", "2016-09-12",
        "2016-04-03")
        .map(LocalDate::parse)
        .any(dt -> dt.getMonthValue() >= 6)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: true
 */

/**
 * When it encountered the 2016-09-12 date, it immediately emitted true and called onComplete().
 * It did not proceed to process 2016-04-03.
 */
```

```java
Observable.range(1,10000)
        .contains(9563)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: true
 */

/**
 * As you can probably guess, the moment the element is found, it will emit true and call onComplete() 
 * and dispose of the operation.
 * If the source calls onComplete() and the element was not found, it will emit false.
 */
```

##### Collecting

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toList()
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: [Alpha, Beta, Gamma, Delta, Epsilon]
 */


/**
 * By default, toList() will use a standard ArrayList implementation.
 */
Observable.range(1,1000)
        .toList(1000)
        .subscribe(s -> System.out.println("Received: " + s));


/**
 * If you want to specify a different list implementation besides ArrayList,
 * you can provide a Callable lambda as an argument to construct one.
 */

Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toList(CopyOnWriteArrayList::new)
        .subscribe(s -> System.out.println("Received: " + s));
```

```java
/**
 * A different flavor of toList() is toSortedList().
 * This will collect the emissions into a list that sorts the items naturally based on their 
 * Comparator implementation. Then, it will emit that sorted List<T> forward to the Observer
 */

Observable.just(6, 2, 5, 7, 1, 4, 9, 8, 3)
        .toSortedList()
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: [1, 2, 3, 4, 5, 6, 7, 8, 9]
 */

/**
 * Like sorted(), you can provide a Comparator as an argument to apply a different sorting logic.
 * You can also specify an initial capacity for the backing ArrayList just like toList().
 */
```

```java
/**
 * the toMap() operator will collect emissions into Map<K,T>,
 * where K is the key type derived off a lambda Function<T,K> argument producing the key for 
 * each emission.
 */

Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toMap(s -> s.charAt(0)).subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: {A=Alpha, B=Beta, D=Delta, E=Epsilon, G=Gamma}
 */

/**
 * To yield a different value other than the emission to associate with the key,
 * provide a second lambda argument that maps each emission to a different value:
 */

Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toMap(s -> s.charAt(0), String::length)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: {A=5, B=4, D=5, E=7, G=5}
 */

/**
 * By default, toMap() will use HashMap. We can use an other map implementation
 */
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toMap(s -> s.charAt(0), String::length, ConcurrentHashMap::new)
        .subscribe(s -> System.out.println("Received: " + s));


/**
 * if I have a key that maps to multiple emissions, the last emission for that key is going 
 * to replace subsequent ones.
 */
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toMap(String::length)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: {4=Beta, 5=Delta, 7=Epsilon}
 */

/**
 * Use toMultiMap() to make a key map to multiple emissions
 */
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .toMultimap(String::length)
        .subscribe(s -> System.out.println("Received: " + s));
/**
 * Received: {4=[Beta], 5=[Alpha, Gamma, Delta], 7=[Epsilon]}
 */
```

```java
/**
 * use the collect() operator to specify a different type to collect items into:
 * -> Specify two arguments that are built with lambda expressions: initialValueSupplier, 
 * and method to use:
 */
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .collect(HashSet::new, HashSet::add)
        .subscribe(s -> System.out.println("Received: " + s));

/**
 * Received: [Gamma, Delta, Alpha, Epsilon, Beta]
 */
```

##### Error Recovery

```java
Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> 10 / i)
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED ERROR: java.lang.ArithmeticException: / by zero
 */
```

```java
Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> 10 / i)
        .onErrorReturnItem(-1)
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> 10 / i)
        .onErrorReturn(e -> - 1)
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED: -1
 */

Observable.just(5, 2, 4, 0, 3, 2, 8)
        .onErrorReturn(e -> - 1)
        .map(i -> 10 / i)
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED ERROR: java.lang.ArithmeticException: / by zero
 */
```

```java
Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> {
            try {
                return 10 / i;
            } catch (ArithmeticException e) {
                return -1;
            }
        })
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED: -1
 * RECEIVED: 3
 * RECEIVED: 5
 * RECEIVED: 1
 */
```

```java
/**
 * Similar to onErrorReturn() and onErrorReturnItem(), onErrorResumeNext() is very similar.
 * The only difference is that it accepts another Observable as a parameter to emit potentially multiple
 * values, not a single value, in the event of an exception.
 */
Observable.just(5, 2, 4, 0, 10)
        .map(i -> 10 / i)
        .onErrorResumeNext(Observable.just(-1).repeat(3))
        .subscribe(i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED: -1
 * RECEIVED: -1
 * RECEIVED: -1
 */

Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> 10 / i)
        .onErrorResumeNext(Observable.empty())
        .subscribe(i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 */

Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> 10 / i)
        .onErrorResumeNext(
                (Throwable e) -> Observable.just(-1).repeat(3)
        )
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED: -1
 * RECEIVED: -1
 * RECEIVED: -1
 */
```

```java
/*Observable.just(5, 2, 4, 0, 3, 2, 8)
        .map(i -> 10 / i)
        .retry()
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );*/
/**
 * The output of the preceding code snippet is as follows:
 *
 * RECEIVED: 5
 * RECEIVED: 2
 * RECEIVED: 2
 * RECEIVED: 5
 * ...
 */

Observable.just(10, 0, 8)
        .map(i -> 10 / i)
        .retry(2)
        .subscribe(
                i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );
/**
 * RECEIVED: 1
 * RECEIVED: 1
 * RECEIVED: 1
 * RECEIVED ERROR: java.lang.ArithmeticException: / by zero
 */
```

##### Action Operations

```java
Observable.just("Alpha", "Beta")
        .doOnNext(s -> System.out.println("Processing: " + s))
        .doAfterNext(s -> System.out.println("Done with: " + s))
        .doOnComplete(() -> System.out.println("Source is done emitting!"))
        .map(String::length)
        .subscribe(i -> System.out.println("Received: " + i));

/**
 * Processing: Alpha
 * Received: 5
 * Done with: Alpha
 * Processing: Beta
 * Received: 4
 * Done with: Beta
 * Source is done emitting!
 */
```

```java
/**
 * And, of course, onError() will peek at the error being emitted up the chain, and you can perform 
 an action with it.This can be helpful to put between operators to see which one is to blame for 
 * an error:
 */

Observable.just(5, 2, 4, 0, 3, 2, 8)
        .doOnError(e -> System.out.println("Source failed!"))
        .map(i -> 10 / i)
        .doOnError(e -> System.out.println("Division failed!"))
        .subscribe(i -> System.out.println("RECEIVED: " + i),
                e -> System.out.println("RECEIVED ERROR: " + e)
        );

/**
 * RECEIVED: 2
 * RECEIVED: 5
 * RECEIVED: 2
 * Division failed!
 * RECEIVED ERROR: java.lang.ArithmeticException: / by zero
 */

/**
 * You can specify all three actions for onNext(), onComplete(), and onError() using doOnEach() as well.
 */
```

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .doOnSubscribe(d -> System.out.println("Subscribing!"))
        .doOnDispose(() -> System.out.println("Disposing!"))
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * Subscribing!
 * RECEIVED: Alpha
 * RECEIVED: Beta
 * RECEIVED: Gamma
 * RECEIVED: Delta
 * RECEIVED: Epsilon
 * Disposing!
 */

Observable.just(5, 3, 7, 10, 2, 14)
        .reduce((total, next) -> total + next)
        .doOnSuccess(i -> System.out.println("Emitting: " + i))
        .subscribe(i -> System.out.println("Received: " + i));

/**
 * Emitting: 41
 * Received: 41
 */
```

##### Combine

```java
Observable<String> source1 = Observable.just("Alpha", "Beta", "Gamma");
Observable<String> source2 = Observable.just("Delta", "Epsilon");

Observable
        .merge(source1, source2)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
 * RECEIVED: Alpha
 * RECEIVED: Beta
 * RECEIVED: Gamma
 * RECEIVED: Delta
 * RECEIVED: Epsilon
 */
System.out.println("---------");

source1
        .mergeWith(source2)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

System.out.println("---------");

Observable.concat(source1, source2)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

System.out.println("---------");

Observable.mergeArray(source1, source2, source1)
        .subscribe(i -> System.out.println("RECEIVED: " + i));

System.out.println("---------");

Observable.merge(List.of(source1, source2, source1))
        .subscribe(i -> System.out.println("RECEIVED: " + i));

/**
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
RECEIVED: Delta
RECEIVED: Epsilon
---------
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
RECEIVED: Delta
RECEIVED: Epsilon
---------
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
RECEIVED: Delta
RECEIVED: Epsilon
---------
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
RECEIVED: Delta
RECEIVED: Epsilon
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
---------
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
RECEIVED: Delta
RECEIVED: Epsilon
RECEIVED: Alpha
RECEIVED: Beta
RECEIVED: Gamma
**/
```

```java
//emit every 1 second
Observable<String> source1 = Observable.interval(1, TimeUnit.SECONDS)
        .map(l -> l + 1) // emit elapsed seconds
        .map(l -> "Source1: " + l + " seconds");

//emit every 300 milliseconds
Observable<String> source2 = Observable.interval(300, TimeUnit.MILLISECONDS)
        .map(l -> (l + 1) * 300) // emit elapsed milliseconds
        .map(l -> "Source2: " + l + " milliseconds");

//merge and subscribe
Observable.merge(source1, source2).subscribe(System.out::println);

//keep alive for 3 seconds
sleep(3000);
```

```java
Observable<String> source = Observable.just("Ali", "Ben M.");
source.flatMap(s -> Observable.fromArray(s.split(""))).subscribe(System.out::println);

/**
 * A
 * l
 * i
 * B
 * e
 * n
 *
 * M
 * .
 */
```

```java
Observable<String> source =
        Observable.just("521934/2342/FOXTROT", "21962/12112/78886/TANGO", "283242/4542/WHISKEY/2348562");

source.flatMap(s -> Observable.fromArray(s.split("/")))
        .filter(s -> s.matches("[0-9]+")) //use regex to filter integers
        .map(Integer::valueOf)
        .subscribe(System.out::println);

/**
 * 521934
 * 2342
 * 21962
 * 12112
 * 78886
 * 283242
 * 4542
 * 2348562
 */
```

```java
Observable<Integer> intervalArguments = Observable.just(2, 3, 10, 7);

intervalArguments
        .flatMap(
            i -> Observable
                    .interval(i, TimeUnit.SECONDS)
                    .map(j -> i + "s interval: " + ((i + 1) * i) + " seconds elapsed")
        ).subscribe(System.out::println);

sleep(12000);
```

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
        .flatMap(s -> Observable.fromArray(s.split("")), (s,r) -> s + "-" + r)
        .subscribe(System.out::println);

/**
 * Alpha-A
 * Alpha-l
 * Alpha-p
 * Alpha-h
 * Alpha-a
 * Beta-B
 * Beta-e
 */
```

```java
Observable<String> source = Observable.just("Alpha", "Beta");
source.concatMap(s -> Observable.fromArray(s.split(""))).subscribe(System.out::println);

/**
 * A
 * l
 * p
 * h
 * a
 * B
 * e
 * ..
 */
```

```java
/**
 * The Observable.amb() factory (amb stands for ambiguous) will accept an Iterable<Observable<T>>
 * and emit the emissions of the first Observable that emits, while the others are disposed of.
 */

/**
 * Here, we have two interval sources and we combine them with the Observable.amb() factory.
 * If one emits every second while the other every 300 milliseconds,
 * the latter is going to win because it will emit first:
 */

//emit every second
Observable<String> source1 = Observable.interval(1, TimeUnit.SECONDS)
                .take(2)
                .map(l -> l + 1) // emit elapsed seconds
                .map(l -> "Source1: " + l + " seconds");


//emit every 300 milliseconds
Observable<String> source2 = Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(l -> (l + 1) * 300) // emit elapsed milliseconds
                .map(l -> "Source2: " + l + " milliseconds");


//emit Observable that emits first
Observable.amb(Arrays.asList(source1, source2)).subscribe(i -> System.out.println("RECEIVED: " + i));

//keep application alive for 5 seconds
sleep(5000);

/**
 * RECEIVED: Source2: 300 milliseconds
 * RECEIVED: Source2: 600 milliseconds
 * RECEIVED: Source2: 900 milliseconds
 * RECEIVED: Source2: 1200 milliseconds
 * RECEIVED: Source2: 1500 milliseconds
 * ...
 */

source1.ambWith(source2);
```

```java
Observable<String> source1 = Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
Observable<Integer> source2 = Observable.range(1,10);
Observable.zip(source1, source2, (s1,s2) -> s1 + "-" + s2).subscribe(System.out::println);

/**
 * Alpha-1
 * Beta-2
 * Gamma-3
 * Delta-4
 * Epsilon-5
 */

source1.zipWith(source2, (s,i) -> s + "-" + i);
```

```java
Observable<String> source = Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
Observable<GroupedObservable<Integer,String>> byLengths = source.groupBy(s -> s.length());
byLengths.flatMapSingle(grp -> grp.toList()).subscribe(System.out::println);

/**
 * [Beta]
 * [Alpha, Gamma, Delta]
 * [Epsilon]
 */
```

```java
Observable<String> source = Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");

Observable<GroupedObservable<Integer,String>> byLengths = source.groupBy(s -> s.length());

byLengths.flatMapSingle(
        g -> g.reduce("", (x,y) -> x.equals("") ? y : x + ", " + y)
                .map(s -> g.getKey() + ": " + s)
).subscribe(System.out::println);

/**
 * 4: Beta
 * 5: Alpha, Gamma, Delta
 * 7: Epsilon
 */
```

##### Multicasting, replay and cache

- multicast is useful to prevent redundant work being done by multiple Observers.
- multicasting creates hot ConnectableObservables, and you have to be careful and time the connect() call so data is not missed by Observers.
- One Observer, it is ok, if there are multiple Observers, you need to find the proxy point where you can multicast and consolidate the upstream operations.

```java
Observable<Integer> src1 = Observable.range(1, 3);
src1.subscribe(i -> System.out.println("Observer One: " + i));
src1.subscribe(i -> System.out.println("Observer Two: " + i));

/**
 * Observer One: 1
 * Observer One: 2
 * Observer One: 3
 * Observer Two: 1
 * Observer Two: 2
 * Observer Two: 3
 * These were two separate streams of data generated for two separate subscriptions.
 */

/**
 * Goal: consolidate them into a single stream of data that pushes each emission
 * to both Observers simultaneously,
 */

ConnectableObservable<Integer> src2 = Observable.range(1, 3).publish();
src2.subscribe(i -> System.out.println("Observer One: " + i));
src2.subscribe(i -> System.out.println("Observer Two: " + i));

src2.connect();

/**
 * Observer One: 1
 * Observer Two: 1
 * Observer One: 2
 * Observer Two: 2
 * Observer One: 3
 * Observer Two: 3
 */
```

```java
Observable<Integer> threeRandoms01 = Observable
        .range(1, 3)
        .map(i -> current().nextInt(100000));

threeRandoms01.subscribe(i -> System.out.println("Observer 1: " + i));
threeRandoms01.subscribe(i -> System.out.println("Observer 2: " + i));

/**
 * Observer 1: 38895
 * Observer 1: 36858
 * Observer 1: 82955
 * Observer 2: 55957
 * Observer 2: 47394
 * Observer 2: 16996
 *
 * range() source will yield two separate emission generators, and each will coldly
 * emit a separate stream for each Observer.
 * Each stream also has its own separate map() instance, hence each Observer gets
 * different random integers.
 */

ConnectableObservable<Integer> cobservableFor02 = Observable
        .range(1, 3)
        .publish(); /***/
Observable<Integer> threeRandoms02 = cobservableFor02.map(i -> current().nextInt(100000));

threeRandoms02.subscribe(i -> System.out.println("Observer 1: " + i));
threeRandoms02.subscribe(i -> System.out.println("Observer 2: " + i));
cobservableFor02.connect();

/**
 * Observer 1: 99350
 * Observer 2: 96343
 * Observer 1: 4155
 * Observer 2: 75273
 * Observer 1: 14280
 * Observer 2: 97638
 * Same result: not the same random
 */

ConnectableObservable<Integer> threeRandoms03 = Observable
        .range(1, 3)
        .map(i -> current().nextInt(100000))
        .publish(); /** need to call publish() after map() instead **/

threeRandoms03.subscribe(i -> System.out.println("Observer 1: " + i));
threeRandoms03.subscribe(i -> System.out.println("Observer 2: " + i));

threeRandoms03.connect();
```

```java
Observable<Integer> threeRandoms = Observable.range(1, 3)
        .map(i -> current().nextInt(100000))
        .publish()
        .autoConnect();

//Observer 1 - print each random integer
threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));

//Observer 2 - sum the random integers, then print
threeRandoms
        .reduce(0, (total, next) -> total + next)
        .subscribe(i -> System.out.println("Observer 2: " + i));


/**
 * .autoConnect(0): it will start firing immediately and not wait for any Observers.
 */

/**
 * .autoConnect(1) or .autoConnect() : when one observer comes in
 * Observer 1: 83428
 * Observer 1: 77336
 * Observer 1: 64970
 */

/**
 * .autoConnect(2) : when 2 observers come in;
 * Observer 1: 83428
 * Observer 1: 77336
 * Observer 1: 64970
 * Observer 2: 225734
 */

/**
 * .autoConnect(3)
 */

/**
 * .autoConnect(2);
 * + 3 observers
 * => 3rd misses data
 */

Observable<Long> seconds = Observable
        .interval(1, TimeUnit.SECONDS)
        .publish()
        .autoConnect(); /** 1st will play it **/

//Observer 1
seconds.subscribe(i -> System.out.println("Observer 1: " + i));
sleep(3000);
//Observer 2
seconds.subscribe(i -> System.out.println("Observer 2: " + i));
sleep(3000);

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
```

```java
/**
 * The refCount() operator on ConnectableObservable is similar to autoConnect(1), which fires after
 * getting one subscription. When it has no Observers anymore, it will dispose of itself and start
 * over when a new one comes in.
 */

Observable<Long> seconds = Observable
        .interval(1, TimeUnit.SECONDS)
        .publish()
        .refCount(); /* = autoConnect(1) + dispose when no consumers*/

//Observer 1
seconds.take(5)
        .subscribe(l -> System.out.println("Observer 1: " + l));

sleep(3000);

//Observer 2
seconds.take(2)
        .subscribe(l -> System.out.println("Observer 2: " + l));

sleep(3000);
//there should be no more Observers at this point

//Observer 3
seconds.subscribe(l -> System.out.println("Observer 3: " + l));

sleep(3000);

/**
 * Observer 1: 0
 * Observer 1: 1
 * Observer 1: 2
 * Observer 1: 3
 * Observer 2: 3
 * Observer 1: 4
 * Observer 2: 4
 * Observer 3: 0
 * Observer 3: 1
 * Observer 3: 2
 */

/**
 * helpful to multicast between multiple Observers but dispose of the upstream connection
 * when no downstream Observers are present anymore
 */

/** publish().refCount() = share()
 * Observable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS).share();
 **/
```

```java
/**
 * Replay;
 * re-emit them when a new Observer comes in
 */

/** replay() all previous emissions to tardy Observers, and then emit current emissions
 * as soon as the tardy Observer is caught up.
 */

Observable<Long> seconds =
        Observable.interval(1, TimeUnit.SECONDS)
                .replay()
                .autoConnect();

//Observer 1
seconds.subscribe(l -> System.out.println("Observer 1: " + l));

sleep(3000);

//Observer 2
seconds.subscribe(l -> System.out.println("Observer 2: " + l));

sleep(3000);

/**
 * Observer 1: 0 <-- Observer 1 came in
 * Observer 1: 1
 * Observer 1: 2
 * Observer 2: 0 <-- Observer 2 came in so source started from 0; this can get expensive with memory
 * Observer 2: 1
 * Observer 2: 2
 * Observer 1: 3 <- same
 * Observer 2: 3 <- same
 * Observer 1: 4
 * Observer 2: 4
 * Observer 1: 5
 * Observer 2: 5
 */

/** replay(n) specify a bufferSize argument to limit only replaying a certain number of last emissions */

/**
 * .replay(2)
 * Observer 1: 0
 * Observer 1: 1
 * Observer 1: 2
 * Observer 2: 1 <- Observer came in; it will not get 0, but it will receive 1 and 2
 * Observer 2: 2
 * Observer 1: 3
 * Observer 2: 3
 * Observer 1: 4
 * Observer 2: 4
 * Observer 1: 5
 * Observer 2: 5
 */

/**
 * Note that if you always want to persist the cached values in your replay() even if there are
 * no subscriptions, use it in conjunction with autoConnect(), not refCount() 
 * => you may consider using autoConnect() to persist the  state of replay() and not have it 
 dispose of when no Observers are present.
 */

Observable<String> source01 =
        Observable.just("a", "b", "c", "d", "e")
                .replay(1)
                .autoConnect();

//Observer 1
source01.subscribe(l -> System.out.println("Observer 1: " + l));
//Observer 2
source01.subscribe(l -> System.out.println("Observer 2: " + l));

/**
 * Observer 1: a
 * Observer 1: b
 * Observer 1: c
 * Observer 1: d
 * Observer 1: e
 * Observer 2: e
 */

Observable<String> source02 =
        Observable.just("a", "b", "c", "d", "e")
                .replay(1)
                .refCount();

/**
 *
 * What happened here is that refCount() causes the cache (and the entire chain) to dispose
 * of and reset the moment Observer 1 is done as there are no more Observers.
 * Observer 1: Alpha
 * Observer 1: Beta
 * Observer 1: Gamma
 * Observer 1: Delta
 * Observer 1: Epsilon
 * Observer 2: Alpha <- Observer 2 came in, it starts over like the first Observer, and another 
 * cache is built.
 * Observer 2: Beta     ==> may consider using autoConnect() to persist the  state of replay()
 * Observer 2: Gamma
 * Observer 2: Delta
 * Observer 2: Epsilon
 */
```

```java
/**
 * to cache all emissions indefinitely for the long term and do not need to control the subscription 
 behavior to the source with ConnectableObservable, you can use the cache() operator.
 */

Observable<Integer> cachedRollingTotals =
        Observable.just(6, 2, 5, 7, 1, 4, 9, 8, 3)
                .scan(0, (total,next) -> total + next)
                .cache();

cachedRollingTotals.subscribe(System.out::println);

/**
 * specify the number of elements to be expected in the cache
 * .cacheWithInitialCapacity(9);
 */
```

