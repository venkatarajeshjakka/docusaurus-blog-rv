---
title: Threads in C#
description: C# Advanced concepts | Asynchronous Programming | Threads
sidebar_label: "Threads in C#"
sidebar_position: 8
---

The execution of multiple task or activity is known as multi tasking.

The execution route of a program is referred as a thread. The control flow is specific to each thread. The use of threads reduces CPU cycle waste and boosts a program's performance.

## Multi-threading

Every program or application is by default a single threaded model since the program's logic is executed by a single thread, know as "Main Thread".

The tern "multi-threading" refers to a method in which a single process is divided into many threads. In this case, each thread does a distinct set of tasks.

- As a result of this, multithreading allows numerous tasks to be completed simultaneously.

## Thread life Cycle

1. **Unstarted state**: It is the state when the thread object is created however the Start method has not yet been invoked.
2. **Ready state**: It is the state when the thread object is ready to be executed and is awating a CPU cycle.
3. **Not Runnable state**:When a thread is not operational

- Sleep function has been invoked
- Wait function has been invoked
- interrupted by I/O operations

4. **Dead state**: It is the state under which a thread accomplishes its function or is terminated.
