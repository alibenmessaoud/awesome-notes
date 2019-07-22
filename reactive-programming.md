# Reactive Programming

Reactive, reactive programming, reactive architecture, etc., there are plenty of explanations and definitions out there on the Internet. Wikipedia, StackOverflow and Reactive Manifesto are not clear. Everybody is talking about events, streams, observables, systems, & architectures, about being or going reactive, how when and why ?! It is not so clear too?

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