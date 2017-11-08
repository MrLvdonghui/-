From a technical standpoint, a thread is a combination of the kernel-level and application-level data structures needed to manage the execution of code. The kernel-level structures coordinate the dispatching of events to the thread and the preemptive scheduling of the thread on one of the available cores. The application-level structures include the call stack for storing function calls and the structures the application needs to manage and manipulate the thread’s attributes and state.


# potential advantages:
* Multiple threads can improve an application’s perceived responsiveness.
* Multiple threads can improve an application’s real-time performance on multicore systems.


## Alternatives to Threads
One problem with creating threads yourself is that they add uncertainty to your code. Threads are a relatively low-level and complicated way to support concurrency in your application. If you do not fully understand the implications of your design choices, you could easily encounter synchronization or timing issues, the severity of which can range from subtle behavioral changes to the crashing of your application and the corruption of the user’s data.

Another factor to consider is whether you need threads or concurrency at all. Threads solve the specific problem of how to execute multiple code paths concurrently inside the same process. There may be cases, though, where the amount of work you are doing does not warrant concurrency. Threads introduce a tremendous amount of overhead to your process, both in terms of memory consumption and CPU time. You may discover that this overhead is too great for the intended task, or that other options are easier to implement

> 上面就是说：1、线程是低层级而且复杂的方式去实现并发，如果你不完全理解的话，很可能遇到同步和计时问题，导致程序崩溃或者破坏数据；2 线程占用了大量的内存消耗和CPU，有其他的方式可以做的更好
> 下面列举出了这些方式
* Operation objects: it hides the thread management aspects of performing the task, leaving you free to focus on the task itself. You typically use these objects in conjunction with an operation queue object, which actually manages the execution of the operation objects on one or more threads. 
* Grand Central Dispatch(GCD): Grand Central Dispatch is another alternative to threads that lets you focus on the tasks you need to perform rather than on thread management. With GCD, you define the task you want to perform and add it to a work queue, which handles the scheduling of your task on an appropriate thread. Work queues take into account the number of available cores and the current load to execute your tasks more efficiently than you could do yourself using threads. 
* Idle-time notifications: For tasks that are relatively short and very low priority, idle time notifications let you perform the task at a time when your application is not as busy
> 使用通知，会在run loop 空闲的时候延迟通知。
* Asynchronous functions: The system interfaces include many asynchronous functions that provide automatic concurrency for you. These APIs may use system daemons and processes or create custom threads to perform their task and return the results to you. (The actual implementation is irrelevant because it is separated from your code.) 
> 系统提供的异步API，具体实现无关紧要，和自己实现的代码是分开的。
* Timers: You can use timers on your application’s main thread to perform periodic tasks that are too trivial to require a thread, but which still require servicing at regular intervals.
> 依赖线程 和 定期维护
* Separate processes： Although more heavyweight than threads, creating a separate process might be useful in cases where the task is only tangentially related to your application. You might use a process if a task requires a significant amount of memory or must be executed using root privileges. For example, you might use a 64-bit server process to compute a large data set while your 32-bit application displays the results to the user. 
> 使用进程，场景: 只和应用程序相关或者需要root权限执行的任务。



# Threading Support 
* cocoa threads: Cocoa implements threads using the NSThread class. Cocoa also provides methods on NSObject for spawning new threads and executing code on already-running threads
> 1、NSThread Class ；2、NSObject 提供的 performSelectorInBackground:withObject: 系列方法。
* POSIX threads:  The POSIX interface is relatively simple to use and offers ample flexibility for configuring your threads.
> ample flexibility 足够灵活去配置线程
* Multiprocessing Services
> Multiprocessing Services 可以忽视，不支持iOS开发。

# Thread Management
At the application level, all threads behave in essentially the same way as on other platforms. After starting a thread, the thread runs in one of three main states: running, ready, or blocked. If a thread is not currently running, it is either blocked and waiting for input or it is ready to run but not scheduled to do so yet. The thread continues moving back and forth among these states until it finally exits and moves to the terminated state.

When you create a new thread, you must specify an entry-point function (or an entry-point method in the case of Cocoa threads) for that thread. This entry-point function constitutes the code you want to run on the thread. When the function returns, or when you terminate the thread explicitly, the thread stops permanently and is reclaimed by the system. Because threads are relatively expensive to create in terms of memory and time, it is therefore recommended that your entry point function do a significant amount of work or set up a run loop to allow for recurring work to be performed. 
> 总结： 1、线程有3个状态： running,ready,blocked；2、线程需要有函数或者方法入口；3、线程开销大，因此建议您的入口点函数执行大量工作或设置运行循环以允许执行重复性工作
> more information,see [Thread Management]()


# Run Loops 
A run loop is a piece of infrastructure used to manage events arriving asynchronously on a thread. A run loop works by monitoring one or more event sources for the thread. As events arrive, the system wakes up the thread and dispatches the events to the run loop, which then dispatches them to the handlers you specify. If no events are present and ready to be handled, the run loop puts the thread to sleep.

You are not required to use a run loop with any threads you create but doing so can provide a better experience for the user. Run loops make it possible to create long-lived threads that use a minimal amount of resources. Because a run loop puts its thread to sleep when there is nothing to do, it eliminates the need for polling, which wastes CPU cycles and prevents the processor itself from sleeping and saving power.

To configure a run loop, all you have to do is launch your thread, get a reference to the run loop object, install your event handlers, and tell the run loop to run. The infrastructure provided by OS X handles the configuration of the main thread’s run loop for you automatically. If you plan to create long-lived secondary threads, however, you must configure the run loop for those threads yourself.
> Details about run loops and examples of how to use them are provided in [Run Loops]().
> 通过监听事件，来唤起线程或者让线程休眠

# Synchronization Tools
* lock
* conditions
> (If you use operation objects, you can configure dependencies among your operation objects to sequence the execution of tasks, which is very similar to the behavior offered by conditions.
* atomic operations(Swift 已经弃用)

> For more information about the available synchronization tools, see [Synchronization Tools]()

# Inter-thread Communication
the fact that threads share the same process space means you have lots of options for communication.
The techniques in this table are listed in order of increasing complexity
* Direct messageing
* Global variables,shared memory and objects
* [conditions](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW4)
* [Run loop sources](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1)
* [Ports and sockets](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1)

> 多线程需要注意的几点
* Avoid Creating Threads Explicitly
* Keep Your Threads Reasonably Busy： If you decide to create and manage threads manually
> Before you start terminating idle threads, you should always record a set of baseline measurements of your applications current performance. After trying your changes, take additional measurements to verify that the changes are actually improving performance, rather than hurting it.
* Avoid Shared Data Structures
> [see Synchronization](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW1)
* Threads and Your User Interface (无非就是注意主线程更新视图)
> For more information about Cocoa thread safety, see [Thread Safety Summary](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-SW1). For more information about drawing in Cocoa, see [Cocoa Drawing Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290).
* Be Aware of Thread Behaviors at Quit Time
> A process runs until all non-detached threads have exited. By default, only the application’s main thread is created as non-detached, but you can create other threads that way as well. When the user quits an application, it is usually considered appropriate behavior to terminate all detached threads immediately, because the work done by detached threads is considered optional. If your application is using background threads to save data to disk or do other critical work, however, you may want to create those threads as non-detached to prevent the loss of data when the application exits
> 就是说 一个进程是否退出和不分离的线程有关，进程退出，分离的线程也被终止，所以一些重要的task应该分配的不分离的线程中，即需要创建不分离的线程。
> For information on creating joinable threads, see Setting the Detached State of a Thread. (only POSIX API support)
* Handle Exceptions
> Each thread has its own call stack, each thread is therefore responsible for catching its own exceptions
* Terminate Your Threads Cleanly
> The best way for a thread to exit is naturally, by letting it reach the end of its main entry point routine. Although there are functions to terminate threads immediately, those functions should be used only as a last resort. Terminating a thread before it has reached its natural end point prevents the thread from cleaning up after itself. If the thread has allocated memory, opened a file, or acquired other types of resources, your code may be unable to reclaim those resources, resulting in memory leaks or other potential problems.For more information on the proper way to exit a thread, see [Terminating a Thread](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW10)

* Thread Safety in Libraries
> 库开发人员需要注意多线程情况下的线程安全，使用库不需要关注，而库开发（架构师吧）需要关注啊！！！




