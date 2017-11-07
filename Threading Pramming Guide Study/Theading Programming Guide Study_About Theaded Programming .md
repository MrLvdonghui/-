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






