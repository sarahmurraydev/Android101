# Coroutines

Notes from: https://medium.com/@korhanbircan/multithreading-and-kotlin-ac28eed57fea 

## Threads: What are they? Why do we use them?

Concurrency 

Computers and phones today have more CPU than the computer used to put a man on the moon. Therefore, modern computer architectures support the ability to execute multiple processes and threads concurrently. Since concurrency is a complicated problem, modern frameworks should have a model in place to handle this and allow developers to take advantage of the CPU capabilities of their device. 

A **process** is running an instance of a program.
A **thread** is the smallest sequence of instructions that can be managed independently by a scheduler.

Each process is divided into one or more threads. 
Multiple threads are interleaved and receive a small amount of time called a quantum. 
When the thread’s quantum is complete, the thread scheduler switches the thread with another one. 

Sequential execution is an inherent part of how humans think, operate, and understand the world around us. We aren’t good at doing several things at once, but instead are good at shifting our attention back and forth between multiple things (similar to thread context switching, suspending, and restoring the state of different sequences). 

### Threads come at a cost
*Myth*: Spawning more threads can help us execute more tasks 
Creating too many threads can cause the app to run slower 
Threads are objects which impose overhead during object allocation and garbage collection 
There’s also a nontrivial expense incurred when switching between them 

For example, think about working a busy job. If you are really swamped and doing a ton of work all the time, you might want to hire new employees (make new threads). This can be beneficial to a certain point, but hiring an infinite amount of employees is not recommended or possible. At a certain point, the benefit of having a new employee will not be worth the cost to you/your company. The new employee requires resources (salary, your time in training), that take away from your company’s overall resources and they may not do a better job than you could have done yourself. 

**Garbage collection** is the process by which Java program performs automatic memory management. See here for more

Now that we understand what threads are, why we might want multiple in our application, but that they don’t come for free, let’s talk about coroutines. 

## Coroutines
Kotlin **coroutines** are a way to write asynchronous, non-blocking code that can be thought of as light weight threads. Similar to threads, coroutines run in parallel or concurrently, wait for and communicate with each other. The biggest difference between coroutines and threads is that creating coroutines is almost free.

For example, let’s say we have some code to generate 1 million threads: 
```kotlin
var counter = 0
val numberOfThreads = 1_000_000
val time = measureTimeMillis {
    for (i in 1..numberOfThreads) {
        thread() {
            counter += 1
        }
    }
}
println("Created ${numberOfThreads} threads in ${time}ms.")
```
This can take ~30 secs to run (try it if you want)

But if we switch from creating our own threads, to instead launching our own coroutines like so: 
```kotlin
var counter = 0
val numberOfCoroutines = 1_000_000
val time = measureTimeMillis {
    for (i in 1..numberOfCoroutines) {
        launch {
            counter += 1
        }
    }
}
```
this can run in ~500ms which is 80x improvement! 

### Why are coroutines much better than threads? 
- As mentioned, the cost to create a coroutine is almost free. 
- Coroutines execute on a shared pool of threads
    * one thread can run many coroutines, 
    * there isn't a need to spawn a new thread for every block of code being executed


### Types of Coroutines: 

**`launch{}`** 
We use `launch{}` when we want to "launch" some work to be done but we don't need to wait on the return of that work 

Launch has the following properties: 
* launches new coroutines without blocking the current thread
* returns a reference to the coroutines as a job object 
* allows the coroutine to be canceled when resulting job is cancelled 
* by default, the coroutine is immediately scheduled for execution

**`async{}`**

