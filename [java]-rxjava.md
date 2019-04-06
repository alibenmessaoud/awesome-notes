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
 * sqlite> insert into contacts (first_name, last_name, email, phone) values ("ali", "bm", "mail", "911");
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