# Operating System
---
### Nama : Muhammad Rizqi Putra Nugroho
### NRP : 3124500044
### Kelas : 1 D3 IT B
---

## 1.1 What are the three main purposes of an operating system?

## 1.2 We have stressed the need for an operating system to make efficient use of the computing hardware. When is it appropriate for the operating system to forsake this principle and to "waste" resources? Why is such a system not really wasteful?

## 1.3 What is the main difficulty that a programmer must overcome in writing an operating system for a real-time environment?

## 1.4 Keeping in mind the various definitions of **operating system**, consider whether the operating system should include applications such as web browsers and mail programs. Argue both that it should and that it should not, and support your answers.

## 1.5 How does the distinction between kernel mode and user mode function as a rudimentary form of protection (security)?

## 1.6 Which of the following instructions should be privileged?

- (a) Set value of timer.
- (b) Read the clock.
- (c) Clear memory.
- (d) Issue a trap instruction.
- (e) Turn off interrupts.
- (f) Modify entries in device-status table.
- (g) Switch from user to kernel mode.
- (h) Access I/O device.

## 1.7 Some early computers protected the operating system by placing it in a memory partition that could not be modified by either the user job or the operating system itself. Describe two difficulties that you think could arise with such a scheme.

## 1.8 Some CPUs provide for more than two modes of operation. What are two possible uses of these multiple modes?

## 1.9 Timers could be used to compute the current time. Provide a short description of how this could be accomplished.

## 1.10 Give two reasons why caches are useful. What problems do they solve? What problems do they cause?  
If a cache can be made as large as the device for which it is caching (for instance, a cache as large as a disk), why not make it that large and eliminate the device?

## 1.11 Distinguish between the client-server and peer-to-peer models of distributed systems.


---
<br/>
---


# Operating System – Answers

Below are concise explanations addressing each of the questions:

---

## 1.1 What are the three main purposes of an operating system?

1. **Resource Management**: The OS manages hardware resources such as the CPU, memory, and I/O devices, ensuring efficient and fair allocation among various programs.

2. **User Interface**: It provides interfaces (command-line or graphical) that allow users to interact with the computer system easily.

3. **Application Support**: The OS offers a platform for software applications to run, providing necessary services and APIs to facilitate their execution.

---

## 1.2 When is it appropriate for the operating system to forsake efficient resource usage and "waste" resources? Why is such a system not really wasteful?

In single-user systems, prioritizing user convenience and responsiveness may lead the OS to "waste" resources. For example, implementing a graphical user interface (GUI) consumes more CPU and memory than a command-line interface but enhances user experience. This approach isn't truly wasteful, as the benefits of improved usability and productivity outweigh the costs of additional resource consumption. :contentReference[oaicite:0]{index=0}

---

## 1.3 What is the main difficulty that a programmer must overcome in writing an operating system for a real-time environment?

The primary challenge is ensuring that the OS can meet strict timing constraints, guaranteeing that critical tasks complete within their deadlines. This requires precise scheduling, minimal interrupt latency, and predictable behavior under all conditions. :contentReference[oaicite:1]{index=1}

---

## 1.4 Should the operating system include applications such as web browsers and mail programs? Argue both for and against, supporting your answers.

**For Inclusion**:

- **Integrated User Experience**: Bundling applications like web browsers and mail programs can provide a seamless and consistent interface, enhancing usability.

- **Immediate Usability**: Users have essential tools available immediately after installing the OS, reducing the need for additional setup.

**Against Inclusion**:

- **Bloat and Resource Usage**: Including numerous applications can lead to a bloated system, consuming more disk space and memory, which may not be necessary for all users.

- **Security and Maintenance**: Bundled applications may introduce security vulnerabilities and require regular updates, increasing maintenance overhead.

- **User Choice**: Users might prefer alternative applications, so bundling could limit their choices or lead to redundancy.

This debate balances the convenience of an out-of-the-box functional system against the desire for a lean, customizable environment. :contentReference[oaicite:2]{index=2}

---

## 1.5 How does the distinction between kernel mode and user mode function as a rudimentary form of protection (security)?

Processors operate in two modes:

- **Kernel Mode**: The OS has unrestricted access to all hardware resources, enabling it to perform critical tasks such as memory management and hardware control. :contentReference[oaicite:3]{index=3}

- **User Mode**: Applications run with restricted access, preventing them from executing privileged instructions or directly interacting with hardware.

This separation ensures that user applications cannot inadvertently or maliciously disrupt system stability or compromise security, as they must request services through controlled system calls to transition into kernel mode. :contentReference[oaicite:4]{index=4}

---

## 1.6 Which of the following instructions should be privileged?

Privileged instructions are those that can impact the system's overall operation and security, and they must be executed in kernel mode.

- **Privileged Instructions**:
  - (a) Set value of timer.
  - (c) Clear memory.
  - (e) Turn off interrupts.
  - (f) Modify entries in device-status table.
  - (g) Switch from user to kernel mode.

- **Non-Privileged Instructions**:
  - (b) Read the clock.
  - (d) Issue a trap instruction (this is used to request services from the OS).
  - (h) Access I/O device (depends on system design, but usually controlled).

---

## 1.7 Difficulties of placing the OS in a protected memory partition

1. **Lack of OS Self-Modifications**:  
   - Many operating systems update their own code dynamically (e.g., for patching or driver updates). A non-modifiable partition prevents this, requiring system reboots or complete reinstallation for updates.

2. **Inefficient Use of Memory**:  
   - If the OS needs more space than allocated, it cannot adjust dynamically, potentially leading to inefficient memory usage or inability to load additional OS features.

---

## 1.8 Two possible uses of multiple CPU modes

1. **Enhanced Security**:  
   - Additional modes (e.g., virtual machine mode) allow guest OS instances to run separately from the host OS, increasing security in virtualization environments.

2. **Granular Privilege Levels**:  
   - Some CPUs implement intermediate privilege levels (e.g., rings in x86 architecture), enabling fine-grained control over access to sensitive system operations and improving security.

---

## 1.9 How timers compute the current time

- The OS maintains a system clock by setting up a timer to generate interrupts at fixed intervals.
- The OS counts these interrupts to track time.
- The current time can be computed based on the number of elapsed timer ticks since a predefined start point.

---

## 1.10 Benefits and drawbacks of caches

**Reasons why caches are useful**:
1. **Faster Data Access**:  
   - Frequently accessed data is stored closer to the CPU, reducing retrieval time.
2. **Reduced Load on Main Memory**:  
   - Less frequent memory accesses decrease the burden on RAM.

**Problems caches solve**:
- They mitigate the speed gap between the CPU and main memory.
- They improve system responsiveness by minimizing slow memory fetches.

**Problems caused by caches**:
- **Inconsistency Issues**: If multiple processors use separate caches, data inconsistency can occur (cache coherence problem).
- **Increased Complexity**: Managing cache policies (e.g., eviction strategies) adds to system design complexity.

**Why not make the cache as large as the device?**
- **Cost**: Large caches require expensive, high-speed memory.
- **Diminishing Returns**: A large cache may not significantly improve performance due to locality of reference principles.
- **Power and Space Constraints**: More cache consumes more power and physical space, making it impractical.

---

## 1.11 Client-Server vs. Peer-to-Peer Models

| Feature        | Client-Server Model | Peer-to-Peer (P2P) Model |
|---------------|--------------------|-------------------------|
| **Structure** | Centralized         | Decentralized |
| **Resource Sharing** | Clients request services from a central server | Each node (peer) shares resources with others |
| **Scalability** | Limited by server capacity | Highly scalable |
| **Failure Handling** | Single point of failure (server) | More fault-tolerant, as no single node is crucial |
| **Example** | Web servers, email servers | BitTorrent, blockchain networks |

---

