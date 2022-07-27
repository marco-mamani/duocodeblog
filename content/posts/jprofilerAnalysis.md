---
title: "JProfiler performance analysis"
date: 2022-07-26T21:58:04-04:00
draft: false
cover: 
   image: img/jprofiler.png
   alt: 'Preformance'
   caption: 'Performance'
tags: ["performance", "jprofiler"]
categories: ["tech"]
---

# JProfiler performance analysis
---
If you work as a developer, you will eventually need to measure your application's performance using JProfiler. A lot of the time, this is left to specialist performance teams or engineers who are very familiar with the tools but are less familiar with the code for which they are attempting to assess performance. Even while this strategy may be effective, it may have shortcomings. especially given that members of the performance team are not the programmers. In light of this, I'd like to dispel a few fallacies around it and explain why a developer should also pay attention to a tool like JProfiler to assess performance.
## Requirements
* JProfiler (standalone or plugin in IntelliJ)
* IntelliJ
* A web application that you can profile

## Understanding JProfiler
First JProfiler is a tool for deciphering what occurs within a running JVM. Before beginning the test, it is important to comprehend JProfiler's fundamental features and how they may be used to enhance performance.
### JProfiler has three different features:
* ***Time profiling*** - evaluates the method-level execution pathways of your program.
![Time Profiling](/img/jprofiler1.png "Time Profiling")
* ***Memory profiling*** - This gives a thorough understanding of how the program uses its heap.
![Memory Profiling](/img/jprofiler2.png "Memory Profiling")
* ***Thread profiling*** - This examines synchronization problems between threads.
![Thread Profiling](/img/jprofiler3.png "Thread Profiling")

JProfiler is a single program that incorporates time, memory, and thread profilers.

## JProfiler Settings
* Data Collection Mode: JProfiler provides two data collection methods: sampling and instrumentation.

* Sampling: Suitable for scenarios that do not require high data collection accuracy. The advantage of this method that it has low impact on system performance. The drawback is that it does not support some features such as method-level statistics.
* Instrumentation: The complete data collection mode that supports high accuracy. The drawback is that it must analyze many classes and it has a relatively heavy impact on the application performance. To reduce the impact, you may want to use it with a filter.

## Startup Modes of the Application
You can specify different parameters for the JProfiler agent to control the startup mode of the application.

* Wait for connection from the JProfiler GUI - The application is started only when JProfiler GUI establishes connection with the profiling agent, and the profiling settings are completed. With this option you can profile the startup phase of your application. The command that you can use to enable this option:

   `-agentpath:<path to native library>=port=8849`

* Start immediately, connect later with the JProfiler GUI - JProfiler GUI establishes connection with the profiling agent when it is necessary, and transmits the profiling settings. This option is flexible, but it cannot profile the startup phase of your application. The command that you can use to enable this option: 

   `-agentpath:<path to native library>=port=8849,nowait`
* Profile offline, JProfiler cannot connect - You have to configure triggers that record data and save snapshots that can be opened with the JProfiler GUI later on. The command that you can use to enable this option: 
   
   `-agentpath:<path to native library>=offline,id=xxx,config=/config.xml`

## Using JProfiler to Diagnose the Application Performance
After completing the profiling settings, you can proceed with the performance diagnosis for Producer. We can clearly view graphs (telemetries) about various metrics, such as memory, GC activities, classes, threads, and the CPU load.

![Telemetry](/img/telemetries.png "Telemetry")

We can make the following assumptions based on this telemetry:
1. A large number of objects are generated during the application running process. These objects have a very short lifecycle, and most of them can be promptly recycled by the garbage collector. They won't cause memory usage to continually grow.
2. As expected, the number of loaded classes increases rapidly during the startup period, and stabilizes afterward.
3. When the application is running, many threads are blocked. Extra attention must be paid to this issue.
At the startup phase of the application, the CPU usage is high. We need to find out why.

### CPU Views
The number of executions, execution time, and call relationships of each method in the application are displayed by CPU views. They are helpful in locating the method that has the most significant impact on the performance of your application.

### Call Tree
Call tree clearly shows the hierarchical call relationships between different methods by using tree graphs. In addition, JProfiler sorts the sub-methods by their total execution time, which allows you to quickly locate key methods.

![Call Tree](/img/callTree.png "Call Tree")

For Producer, the method `SendProducerBatchTask.run()` takes most of the time. If you continue to look down, you will find that most of the time is taken by executing the method `Client.PutLogs()`.

### Hot Spots
If you have many application methods, and many of your sub-methods are executed in a short interval, the Hot Spots view can help you quickly locate performance problems. This view can sort the methods based on various factors, such as their individual execution time, total execution time, average execution time, and number of calls. The individual execution time is equal to the total execution time of the method minus the total execution time of all sub-methods.

![HotSpot](/img/hotSpots.png "HotSpot")

In this view, you can see that the following three methods: `Client.PutLogs()`, `LogGroup.toByteArray()`, and `SamplePerformance$1.run()` take the most time to be executed individually.

