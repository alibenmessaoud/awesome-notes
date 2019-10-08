##### Multi-tasking

Standard way in Java: Use threads. 

Native: These threads directly map to threads of execution on the computer CPU cores. This includes java.lang.Thread, java.util.concurrent.Executor, java.util.concurrent.ExecutorService, and so on. 

Green threads: They do not directly map to operating system threads. Instead, the underlying architecture manages the threads itself and manages how these map on to operating system threads. Green threads are a form of preemptive multitasking. 

Fibers: similar to green threads. The big difference between green threads and fibers is in the level of control, and specifically who is in control. This puts us in direct control over when the fibers can pause execution, instead of the system deciding this for us.

> Quasar: Java library that works well with pure Java 11 and Kotlin. It works by having a Java agent that needs to run alongside the application, and this agent is responsible for managing the fibers and ensuring that they work together correctly.

> Kilim: Kilim is a Java library that offers very similar functionality to Quasar but does so by using bytecode weaving instead of a Java agent. 

> Loom: add fibers to the JVM itself, rather than as an add-on library. 

Co-Routines: Co-routines are an alternative to threading and fibers. We can think of co-routines as fibers without any form of scheduling. Instead of the underlying system deciding which tasks are performing at any time, our code does this directly. Generally, we write co-routines so that they yield at specific points of their flow. These can be seen as pause points in our function, where it will stop working and potentially output some intermediate result. When we do yield, we are then stopped until the calling code decides to re-start us for whatever reason. This means that our calling code controls the scheduling of when this will run.