# Nama : Muhammad Rizqi Putra Nugroho 
# NRP : 3124500044
# Kelas : 1 D3 IT B

<br>

# Exercise 4

## 4.1 Provide three programming examples in which multithreading provides better performance than a single-threaded solution.

Here are three examples where multithreading typically offers better performance:

1.  **Web Server:** A web server needs to handle multiple client requests concurrently. Using multiple threads, the server can handle each incoming request (e.g., fetching a web page, processing form data) in a separate thread. A single-threaded server would have to process requests sequentially, making users wait longer, especially if some requests involve time-consuming operations. Multithreading allows for simultaneous request handling, improving throughput and responsiveness.
2.  **Applications with Responsive User Interfaces (GUIs):** In a desktop or mobile application, performing long-running tasks (like saving a large file, complex calculations, or network requests) on the main UI thread would freeze the interface, making it unresponsive to user input. By offloading these tasks to background threads, the main thread remains free to handle UI events (button clicks, scrolling), ensuring the application stays responsive.
3.  **Parallel Data Processing and Scientific Computing:** Tasks involving large datasets or complex computations that can be broken down into independent sub-tasks are well-suited for multithreading. Examples include:
    * Processing large images where different sections can be processed concurrently.
    * Performing matrix operations where rows or columns can be calculated in parallel.
    * Running simulations (like Monte Carlo simulations) where multiple independent simulation runs can occur simultaneously.
    Each thread can work on a portion of the data or a separate computational task, significantly speeding up the overall process on multi-core processors.

## 4.2 Using Amdahl's Law, calculate the speedup gain of an application that has a 60 percent parallel component for (a) two processing cores and (b) four processing cores.

Amdahl's Law is used to predict the theoretical maximum speedup for a program when using multiple processors. The formula is:

$Speedup = \frac{1}{(1 - P) + \frac{P}{N}}$

Where:
* $P$ = the proportion of the application that can be parallelized.
* $(1 - P)$ = the proportion of the application that must remain serial.
* $N$ = the number of processing cores.

Given:
* Parallel component ($P$) = 60% = $0.60$
* Therefore, the serial component ($1 - P$) = $1 - 0.60 = 0.40$

**(a) For two processing cores ($N = 2$):**

$Speedup = \frac{1}{(1 - 0.60) + \frac{0.60}{2}}$
$Speedup = \frac{1}{0.40 + 0.30}$
$Speedup = \frac{1}{0.70}$
$Speedup \approx 1.4286$

**The speedup gain with two cores is approximately $1.43$.**

**(b) For four processing cores ($N = 4$):**

$Speedup = \frac{1}{(1 - 0.60) + \frac{0.60}{4}}$
$Speedup = \frac{1}{0.40 + 0.15}$
$Speedup = \frac{1}{0.55}$
$Speedup \approx 1.8182$

**The speedup gain with four cores is approximately $1.82$.**

## 4.3 Does the multithreaded web server described in Section 4.1 exhibit task or data parallelism?

The multithreaded web server described primarily exhibits **task parallelism**.

**Explanation:**
Each thread handles a separate client request. Although the overall function (serving web content) is the same, the specific tasks performed for each request are often different (e.g., fetching different files, executing different server-side scripts, querying databases with different parameters). The parallelism comes from performing multiple *distinct tasks* (client requests) concurrently, rather than performing the exact same operation on different subsets of data.

## 4.4 What are two differences between user-level threads and kernel-level threads? Under what circumstances is one type better than the other?

**Two Differences:**

1.  **Management & Kernel Awareness:** User-level threads (ULTs) are managed entirely by a thread library in user space, and the kernel is unaware of their existence; it only sees the single process containing them. Kernel-level threads (KLTs) are managed directly by the operating system kernel, and the kernel is aware of each thread.
2.  **Blocking & Scheduling:** If a ULT performs a blocking system call (like I/O), the entire process (including all other ULTs within it) typically blocks, as the kernel only sees the process as blocked. If a KLT blocks, the kernel can schedule another KLT from the same process (or a different process) to run on the available processor core. KLTs can run in parallel on multiple cores, whereas standard ULTs within a single process cannot.

**Circumstances Favoring Each Type:**

* **User-Level Threads (ULTs) are better when:**
    * The application requires a very large number of threads, and the overhead of kernel thread creation/management is too high.
    * Context switching between threads needs to be extremely fast (as it doesn't involve the kernel).
    * The application can be designed to use non-blocking I/O or asynchronous operations effectively managed by the user-level thread library to prevent the whole process from blocking.
    * Portability is a concern, as they don't rely on specific kernel thread support (though the library itself might).
    * True parallelism on multi-core systems is not a primary requirement.

* **Kernel-Level Threads (KLTs) are better when:**
    * The application needs to take advantage of multiple processor cores for true parallel execution.
    * Threads are likely to perform blocking operations (like I/O), and it's important that other threads within the same process can continue executing.
    * Fine-grained scheduling control by the OS (e.g., thread priorities) is needed.
    * They are the standard model on most modern operating systems (like Windows, Linux, macOS), making them easier to use with system libraries and features.

## 4.5 Describe the actions taken by a kernel to context-switch between kernel-level threads.

When a kernel performs a context switch between two kernel-level threads (belonging to the same process or different processes), it takes the following actions:

1.  **Save Context of Old Thread:** The kernel saves the CPU state of the currently running thread. This includes:
    * Program Counter (instruction pointer)
    * CPU Registers (general-purpose registers, floating-point registers, etc.)
    * Stack Pointer
    This state is typically saved into the thread's Kernel Thread Control Block (TCB).
2.  **Update Thread State:** The state of the outgoing thread is updated (e.g., from "running" to "ready" if its time slice expired, or to "waiting" if it blocked on I/O or a synchronization primitive). It might be moved to the appropriate queue (ready queue, wait queue).
3.  **Select Next Thread:** The kernel's scheduler selects the next thread to run from the ready queue based on its scheduling algorithm (e.g., priority-based, round-robin).
4.  **Load Context of New Thread:** The kernel loads the previously saved CPU state of the selected incoming thread from its TCB. This includes restoring the program counter, CPU registers, and stack pointer.
5.  **Update Thread State:** The state of the incoming thread is updated (typically from "ready" to "running").
6.  **Resume Execution:** The CPU control is transferred to the newly loaded thread, which resumes execution at the instruction indicated by the restored program counter.

## 4.6 What resources are used when a thread is created? How do they differ from those used when a process is created?

**Resources Used When a Thread is Created:**

When a thread (within an existing process) is created, the primary resources allocated are:

1.  **Thread Stack:** Each thread requires its own private stack to manage local variables, function call parameters, and return addresses.
2.  **Thread Control Block (TCB):** A data structure (managed by the OS for KLTs, or the library for ULTs) to store thread-specific information like:
    * Thread ID
    * CPU Register values (when not running)
    * Program Counter (when not running)
    * Stack Pointer
    * Thread State (running, ready, waiting, etc.)
    * Scheduling Priority
    * Pointers to the containing process.
3.  **Register Set Storage:** Space (often within the TCB or managed alongside it) to save the CPU registers when the thread is switched out.

**Differences from Process Creation:**

Creating a thread is significantly **less resource-intensive** (lightweight) than creating a process (heavyweight). The key differences lie in what is **shared** versus what is **newly allocated**:

* **Process Creation Requires:**
    * **New Address Space:** A completely separate virtual memory space (code, data, heap segments). This is a major resource allocation.
    * **Process Control Block (PCB):** Contains information about the entire process (PID, state, memory map, open file list, etc.).
    * **Duplicated/Initialized OS Resources:** Such as file descriptor tables (often initially inheriting copies from the parent), security credentials, environment variables, etc.
    * **Resources for at least one initial thread:** Every process starts with at least one thread, so process creation implicitly includes the resources needed for that first thread (stack, TCB).

* **Thread Creation Within an Existing Process:**
    * **Shares Address Space:** Threads within the same process share the *same* virtual address space (code, data, heap).
    * **Shares OS Resources:** Threads share the process's open files, global variables, static data, and other OS resources.
    * **Requires Only Thread-Specific Resources:** Primarily just the stack and TCB mentioned earlier.

In essence, creating a process involves setting up a whole new environment, while creating a thread involves setting up a new execution context *within* an existing environment.

## 4.7 Assume that an operating system maps user-level threads to the kernel using the many-to-many model and that the mapping is done through LWPs. Furthermore, the system allows developers to create real-time threads for use in real-time systems. Is it necessary to bind a real-time thread to an LWP? Explain.

**Yes, it is necessary (or at least highly advisable and common practice) to bind a real-time user-level thread to a specific LWP.**

**Explanation:**

1.  **Predictability:** Real-time systems require predictable performance and timely responses to events. Real-time threads often have strict deadlines.
2.  **Many-to-Many Model Issues:** In a standard many-to-many model without binding, a user-level thread doesn't have a permanent connection to a specific LWP.
    * **Multiplexing:** The user-level thread scheduler might map the real-time thread onto different LWPs over time, or it might run other non-real-time user threads on the same LWP when the real-time thread is not actively running (but perhaps ready).
    * **Blocking:** If the LWP currently executing the real-time user thread is used to execute another user thread that performs a blocking system call, the LWP (and thus the real-time thread, even if it's ready) will block.
    * **Scheduling Interference:** The kernel schedules LWPs, not user threads directly. If a real-time user thread shares an LWP with lower-priority user threads, the kernel cannot give the real-time thread preferential scheduling treatment over the other user threads mapped to that LWP; it only sees the LWP.
3.  **Benefits of Binding:** Binding a real-time user-level thread to a dedicated LWP overcomes these issues:
    * **Dedicated Kernel Resource:** The real-time thread effectively gets its own kernel-schedulable entity (the LWP).
    * **Kernel Scheduling:** The kernel can now apply real-time scheduling policies directly to that LWP (e.g., assigning it a high priority) because it knows this LWP is dedicated to a real-time task.
    * **Avoids Blocking Interference:** The bound LWP won't be blocked by other unrelated user-level threads making system calls.
    * **Ensures Prompt Execution:** When the real-time user thread becomes ready, its dedicated LWP can be scheduled by the kernel promptly (subject to kernel scheduling policies and LWP priority), without interference from the user-level scheduler needing to map it or competing non-real-time user threads sharing the LWP.

Therefore, binding ensures that the real-time thread gets the necessary priority and predictable access to CPU resources via its dedicated LWP, which is crucial for meeting real-time requirements.
