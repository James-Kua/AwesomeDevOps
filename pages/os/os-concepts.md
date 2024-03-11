---
title: "OS Concepts"
---

# OS Concepts

## Operating System

An Operating System (OS) is a collection of software that manages computer hardware and provides services for programs. Specifically, it hides hardware complexity, manages computational resources, and provides isolation and protection. Most importantly, it directly has privilege access to the underlying hardware. Major components of an OS are file system, scheduler, and device driver. You probably have used both Desktop (Windows, Mac, Linux) and Embedded (Android, iOS) operating systems before.

There are 3 key elements of an operating system, which are: (1) Abstractions (process, thread, file, socket, memory), (2) Mechanisms (create, schedule, open, write, allocate), and (3) Policies (LRU, EDF)

There are 2 operating system design principles, which are: (1) Separation of mechanism and policy by implementing flexible mechanisms to support policies, and (2) Optimize for common case: Where will the OS be used? What will the user want to execute on that machine? What are the workload requirements?

The 3 types of Operating Systems commonly used nowadays are: (1) Monolithic OS, where the entire OS is working in kernel space and is alone in supervisor mode; (2) Modular OS, in which some part of the system core will be located in independent files called modules that can be added to the system at run time; and (3) Micro OS, where the kernel is broken down into separate processes, known as servers. Some of the servers run in kernel space and some run in user-space.

## Boot Process

When a system is first booted, or is reset, the processor executes code at a well-known location. In a personal computer (PC), this location is in the basic input/output system (BIOS), which is stored in flash memory on the motherboard. The central processing unit (CPU) in an embedded system invokes the reset vector to start a program at a known address in flash/ROM. In either case, the result is the same. Because PCs offer so much flexibility, the BIOS must determine which devices are candidates for boot. 

When a boot device is found, the first-stage boot loader is loaded into RAM and executed. This boot loader is less than 512 bytes in length (a single sector), and its job is to load the second-stage boot loader.

When the second-stage boot loader is in RAM and executing, a splash screen is commonly displayed, and Linux and an optional initial RAM disk (temporary root file system) are loaded into memory. When the images are loaded, the second-stage boot loader passes control to the kernel image and the kernel is decompressed and initialized. At this stage, the second-stage boot loader checks the system hardware, enumerates the attached hardware devices, mounts the root device, and then loads the necessary kernel modules. When complete, the first user-space program (init) starts, and high-level system initialization is performed.

### System startup

The system startup stage depends on the hardware that Linux is being booted on. On an embedded platform, a bootstrap environment is used when the system is powered on, or reset. Examples include U-Boot, RedBoot, and MicroMonitor from Lucent. Embedded platforms are commonly shipped with a boot monitor. These programs reside in special region of flash memory on the target hardware and provide the means to download a Linux kernel image into flash memory and subsequently execute it. In addition to having the ability to store and boot a Linux image, these boot monitors perform some level of system test and hardware initialization. In an embedded target, these boot monitors commonly cover both the first- and second-stage boot loaders.

In a PC, booting Linux begins in the BIOS at address 0xFFFF0. The first step of the BIOS is the power-on self test (POST). The job of the POST is to perform a check of the hardware. The second step of the BIOS is local device enumeration and initialization.

Given the different uses of BIOS functions, the BIOS is made up of two parts: the POST code and runtime services. After the POST is complete, it is flushed from memory, but the BIOS runtime services remain and are available to the target operating system.

To boot an operating system, the BIOS runtime searches for devices that are both active and bootable in the order of preference defined by the complementary metal oxide semiconductor (CMOS) settings. A boot device can be a floppy disk, a CD-ROM, a partition on a hard disk, a device on the network, or even a USB flash memory stick.

Commonly, Linux is booted from a hard disk, where the Master Boot Record (MBR) contains the primary boot loader. The MBR is a 512-byte sector, located in the first sector on the disk (sector 1 of cylinder 0, head 0). After the MBR is loaded into RAM, the BIOS yields control to it.

### Stage 1 boot loader

The primary boot loader that resides in the MBR is a 512-byte image containing both program code and a small partition table (see Figure 2). The first 446 bytes are the primary boot loader, which contains both executable code and error message text. The next sixty-four bytes are the partition table, which contains a record for each of four partitions (sixteen bytes each). The MBR ends with two bytes that are defined as the magic number (0xAA55). The magic number serves as a validation check of the MB


The job of the primary boot loader is to find and load the secondary boot loader (stage 2). It does this by looking through the partition table for an active partition. When it finds an active partition, it scans the remaining partitions in the table to ensure that they’re all inactive. When this is verified, the active partition’s boot record is read from the device into RAM and executed.


### Stage 2 boot loader

The secondary, or second-stage, boot loader could be more aptly called the kernel loader. The task at this stage is to load the Linux kernel and optional initial RAM disk

The first- and second-stage boot loaders combined are called Linux Loader (LILO) or GRand Unified Bootloader (GRUB) in the x86 PC environment. Because LILO has some disadvantages that were corrected in GRUB, let’s look into GRUB.

The great thing about GRUB is that it includes knowledge of Linux file systems. Instead of using raw sectors on the disk, as LILO does, GRUB can load a Linux kernel from an ext2 or ext3 file system. It does this by making the two-stage boot loader into a three-stage boot loader. Stage 1 (MBR) boots a stage 1.5 boot loader that understands the particular file system containing the Linux kernel image. Examples include reiserfs_stage1_5 (to load from a Reiser journaling file system) or e2fs_stage1_5 (to load from an ext2 or ext3 file system). When the stage 1.5 boot loader is loaded and running, the stage 2 boot loader can be loaded.

With stage 2 loaded, GRUB can, upon request, display a list of available kernels (defined in /etc/grub.conf, with soft links from /etc/grub/menu.lst and /etc/grub.conf). You can select a kernel and even amend it with additional kernel parameters. Optionally, you can use a command-line shell for greater manual control over the boot process.

With the second-stage boot loader in memory, the file system is consulted, and the default kernel image and initrd image are loaded into memory. With the images ready, the stage 2 boot loader invokes the kernel image.

### Kernel

With the kernel image in memory and control given from the stage 2 boot loader, the kernel stage begins. The kernel image isn’t so much an executable kernel, but a compressed kernel image. Typically this is a zImage (compressed image, less than 512KB) or a bzImage (big compressed image, greater than 512KB), that has been previously compressed with zlib. At the head of this kernel image is a routine that does some minimal amount of hardware setup and then decompresses the kernel contained within the kernel image and places it into high memory. If an initial RAM disk image is present, this routine moves it into memory and notes it for later use. The routine then calls the kernel and the kernel boot begins.

### Init

After the kernel is booted and initialized, the kernel starts the first user-space application. This is the first program invoked that is compiled with the standard C library. Prior to this point in the process, no standard C applications have been executed.

In a desktop Linux system, the first application started is commonly /sbin/init. But it need not be. Rarely do embedded systems require the extensive initialization provided by init (as configured through /etc/inittab). In many cases, you can invoke a simple shell script that starts the necessary embedded applications.


## Process and Process Management

A process is an instance of a program being executed (a running program). They allow for a single processor to run multiple programs "simulatneously". Each process runs on a virtual CPU, and the real CPU can use this to run any single process. Effectively, the single CPU has been divided into multiple virtual CPUs.

## Why have Processes?

They:
- provide an illusion of concurrency.
- provide isolation between programs (each process has its own address space).
- simplify programming (programs don't have to worry about other programs).
- allow for better utilisation of resources (different processes require different resources at certain times).

## Time-Slicing for Concurrency

An OS can switch the current running process every few milliseconds such that every process can have CPU time. This is done using a CPU scheduler, and it can be used to provide pseudo concurrency.

## Types of Concurrency

**Pseudo Concurrency**: a single processor switches between processes by interleaving their runtimes. Overtime this can give the illusion of concurrent execution.

**Real Concurrency**: multiple processors or CPU cores are used to run multiple processes in parallel.

## Fairness

Fairness is the concept that the CPU scheduler switches fairly between processes. This means that every process should have equal CPU time, or more generally they should have appropriate CPU times determined by each processes priority and workload.

## Context Switches

A context switch is when a processor switches from executing one process to another.

An OS may switch either periodically or in reponse to certain events / interrupts (such as I/O completion). Context switches are not pre-determined because the events causing them are non-deterministic.

Since processes will need to be restarted safely when being switched to, all relevant information (e.g. register values) must be stored when they are switched away from. This is done by storing the data in a process descriptor or a process control block (PCB), which is kept inside a process table.

Context switches are expensive. The process state needs to be saved / restored and CPU caches (including translation lookaside buffer (TLB)) will be lost. It is important for an OS to avoid unnecessary context switches.

## Process Control Block (PCB)

Each process has it's own virtual machine: They have their own virtual CPU, address space (stack, heap, text, data, etc), open file descriptors, etc. Enough data must be stored such that all of this remains preserved after a context switch.

The information that gets stored on a context switch is:

- **Registers**: program counter (PC), page table register, stack pointer, etc.
- **Process management info**: process ID (PID), parent process, process group, priority, CPU used, etc.
- **File management info**: root directory, working directory, open file descriptors, etc.

## Process Creation

There are two types of processes:

- **Foreground**: processes that interact with users.
- **Background**: daemons (e.g. handling incoming mail, printing requests, etc).

Processes are created on:

- system initialisation.
- user request.
- system calls by a running process.

Processes terminated can be caused by:

- **Normal Completion**: when the process completes the execution of its body.
- **System Calls**: e.g. `exit()` in UNIX, `ExitProcess()` in Windows.
- **Abnormal Exit**: when the process runs into an error or unhandled exception.
- **Aborted**: when another process overrules its execution (e.g. killed from terminall).
- **Never**: some processes run in an endless loop and never terminate unless an error occurs (e.g. most web servers).

## Process Hierarchies

UNIX allows for processes to form hierarchies (e.g. parent, child, child's child, etc).

Windows has no notion of process hierarchy. Instead when a process is created its parent is given a handle token to control it. This handle can be passed around to other processes.

## Linux Signals

In Linux, every signal has a name that begins with characters SIG. For example :

- A SIGINT signal that is generated when a user presses ctrl+c. This is the way to terminate programs from terminal.
- A SIGALRM  is generated when the timer set by alarm function goes off.
- A SIGABRT signal is generated when a process calls the abort function.
etc

When the signal occurs, the process has to tell the kernel what to do with it.  There can be three options through which a signal can be disposed:

- The signal can be ignored. By ignoring we mean that nothing will be done when signal occurs. Most of the signals can be ignored but signals generated by hardware exceptions like divide by zero, if ignored can have weird consequences. Also, a couple of signals like SIGKILL and SIGSTOP cannot be ignored.

- The signal can be caught. When this option is chosen, then the process registers a function with kernel. This function is called by kernel when that signal occurs. If the signal is non fatal for the process then in that function the process can handle the signal properly or otherwise it can chose to terminate gracefully.
Let the default action apply. Every signal has a default action. This could be process terminate, ignore etc.

- As we already stated that two signals SIGKILL and SIGSTOP cannot be ignored. This is because these two signals provide a way for root user or the kernel to kill or stop any process in any situation .The default action of these signals is to terminate the process. Neither these signals can be caught nor can be ignored.

### What Happens at Program Start-up?

It all depends on the process that calls exec. When the process is started the status of all the signals is either ignore or default. Its the later option that is more likely to happen unless the process that calls exec is ignoring the signals.

It is the property of exec functions to change the action on any signal to be the default action. In simpler terms, if parent has a signal catching function that gets called on signal occurrence then  if that parent execs a new child process, then this function has no meaning in the new process and hence the disposition of the same signal is set to the default in the new process.

Also, Since we usually have processes running in background so the shell just sets the quit signal disposition as ignored since we do not want the background processes to get terminated by a user pressing a ctrl+c key because that defeats the purpose of making a process run in background.

## Threads and Concurrency

A thread is an execution stream that share the same address space. When multithreading is used, each process can contain one or more threads.

A break down of per process and per thread items is:

Per process items:

- Address space.
- Global variables.
- Open files.
- Child processes.
- Signals.

Per thread items:

- Program counter (PC).
- Registers.
- Stack.

## Benefits of Multi-threading

- Parallelization: In multi-processor architecture, different threads can execute different instructions at a time, which result in parallelization which speeds up the execution of the process.

- Specialization: By specializing different threads to perform the different task, we can manage threads, for example, we can give higher priority to those threads which are executing the more important task. Also in multi-processor architecture specialization leads to the hotter cache which improves performance.

- Efficient: Multi-threaded programs are more efficient as compared to multi-process programs in terms of resource requirement as threads share address space while process does not, so multi-process programs will require separate memory space allocation. Also, Multi-threaded programs incur lower overhead as inter-thread communication is less expensive.

- Hide Latency: As context switching among the process is a costly operation, as all the threads of a process share the same virtual to physical address mapping and other resources, context switch among the thread is a cheap operation and require less time. When the number of thread is greater than the number of CPU and a thread is in idle state (spending the time to wait for the result of some interrupt) and its idle time is greater than two times the time required for switching the context to another thread, it will switch the switch context to another thread to hide idling time.

## Problems / Concerns with Threads

+ Threads have a shared address space. This means that they don't have any protection from reading / writing to each others stack like processes have. This can cause memory corruption issues and other concurrency bugs due to concurrent access to shared data (e.g. through global variables).

+ Forking inside a thread is another concern. Does the fork create a new process with the same number of threads, or with a single thread?

+ Handling signals also becomes an issue. Which thread should handle the signal?

##  Scheduling

Linux scheduling is based on the time sharing technique: several processes run in “time multiplexing” because the CPU time is divided into slices, one for each runnable process. If a currently running process is not terminated when its time slice or quantum expires, a process switch may take place. Time sharing relies on timer interrupts and is thus transparent to processes. No additional code needs to be inserted in the programs to ensure CPU time sharing.

The Linux scheduler is a priority based scheduler that schedules tasks based upon their static and dynamic priorities. When these priorities are combined they form a task's goodness . Each time the Linux scheduler runs, every task on the run queue is examined and its goodness value is computed. The task with the highest goodness is chosen to run next.

When there are cpu bound tasks running in the system, the Linux scheduler may not be called for intervals of up to .40 seconds. This means that the currently running task has the CPU to itself for periods of up to .40 seconds (how long depends upon the task's priority and whether it blocks or not). This is good for throughput because there are few computationally uneccessary context switches. However it can kill interactivity because Linux only reschedules when a task blocks or when the task's dynamic priority (counter) reaches zero. Thus under Linux's default priority based scheduling method, long scheduling latencies can occur.

Looking at the scheduling latency in finer detail, the Linux scheduler makes use of a timer that interrupts every 10 msec. This timer erodes the currently running task's dynamic priority (decrements its counter). A task's counter starts out at the same value its priority contains. Once its dynamic priority (counter) has eroded to 0 it is again reset to that of its static priority (priority). It is only after the counter reaches 0 that a call to schedule() is made. Thus a task with the default priority of 20 may run for .200 secs (200 msecs) before any other task in the system gets a chance to run. A task at priority 40 (the highest priority allowed) can run for .400 secs without any scheduling occurring as long as it doesn't block or yield. 

Process scheduling is an essential part of a Multiprogramming operating systems. Such operating systems allow more than one process to be loaded into the executable memory at a time and the loaded process shares the CPU using time multiplexing.

## Memory Management

The process address space is the set of logical addresses that a process references in its code. For example, when 32-bit addressing is in use, addresses can range from 0 to 0x7fffffff; that is, 2³¹ possible numbers, for a total theoretical size of 2 gigabytes.

The operating system takes care of mapping the logical addresses to physical addresses at the time of memory allocation to the program. There are three types of addresses used in a program before and after memory is allocated:

- Symbolic addresses: The addresses used in a source code. The variable names, constants, and instruction labels are the basic elements of the symbolic address space.

- Relative addresses: At the time of compilation, a compiler converts symbolic addresses into relative addresses.

- Physical addresses: The loader generates these addresses at the time when a program is loaded into main memory.

Virtual and physical addresses are the same in compile-time and load-time address-binding schemes. Virtual and physical addresses differ in execution-time address-binding scheme.

The set of all logical addresses generated by a program is referred to as a logical address space. The set of all physical addresses corresponding to these logical addresses is referred to as a physical address space.

## Paging

Paging allows for the logical address space to be noncontiguous, so a process can allocate physical memory when it is needed / available.

Paging has two important concepts, **frames** and **pages**:

- **Frames**: a fixed-size block of physical memory.
- **Pages**: a block of logical (virtual) memory, the same size as a frame.

A typical page size is 4KB.

When a program of n pages is run:

1. n free frames are found.
2. the program is loaded into this memory.
3. a **page table** is setup to translate logical addresses to physical addresses.

## Page Table

A page table is a translation mechanism from logical memory to physical memory.

```

    Virtual        Page         Physical
    Memory         Table         Memory
  *--------*       *---*       *--------*
  | Page 0 |     0 | 1 |     0 |        |
  *--------*       *---*       *--------*
  | Page 1 |     1 | 4 |     1 | Page 0 |
  *--------*       *---*       *--------*
  | Page 2 |     2 | 3 |     2 |        |
  *--------*       *---*       *--------*
  | Page 3 |     3 | 7 |     3 | Page 2 |
  *--------*    /  *---*       *--------*
               /     |       4 | Page 1 |
              /      |         *--------*
           Page      |       5 |        |
          Number     |         *--------*
                     *       6 |        |
                    /          *--------*
               Frame ------- 7 | Page 3 |
               Number          *--------*

```

## Address Translation

When translating an address, the page table can be used to find the frame that the data exists in, but the offset inside the frame is also required. This offset is equal to the offset inside the page as they are equal size.

Logical addresses can be split into two sections:

- **Page number** `p`: used as the index to the page table. The page table only stores the base address of the page in physical memory.
- **Page offset** `d`: combined with the base address of the page, the full physical address can be sent to the memory unit.

Given a logical address space `2^m` and page size `2^n`:

```

    Page number | Page offset
  *-------------*-------------*
  |      p      |      d      |
  *-------------*-------------*
     m-n bits       n bits

```

For example, if `m = 32` and `n = 12`, the address `0x12345678` has a page number of `0x12345` and an offset of `0x678`.

## Free Frames

To keep track of which frames are free, we can use a free frames list. Whenever a process needs to allocate new pages, this list can be searched for free frames, which can be removed and entered into the process' page table.

## Memory Protection

To protect from invalid accesses of logical memory (e.g. reading from an unallocated page), each entry in the page table is associated with a **valid-invalid protection bit**.

- **Valid**: indicates a legal page (page is in the process' logical address space).
- **Invalid**: indicates the page is missing. This can be due to:
  - the page is not in the process' logical address space (page fault).
  - needing to load the page from disk (see demand paging).
  - an incorrect access (e.g. a programming error).

## Page Table Implementation

Page tables can be quite large, so as a result it must be kept in main memory. Typically, the MMU will have two registers:

- **Page-table base register (PTBR)**: points to the base of the page table.
- **Page-table length register (PTLR)**: stores the size of the page table.

However, this implementation has a large problem: it is inefficient. Every data / instruction access will require two memory access: one for the page table and one for the data / instruction.

### Associative Memory

To solve this ineffciency, a fast-lookup cache can be used. This cache is implemented in hardware as associative memory, and it can be thought of as a mapping from page number to frame numbers. 

```

    Page number   Frame number
  *-------------*--------------*
  |      0      |      4       |
  *-------------*--------------*
  |      1      |      2       |
  *-------------*--------------*
  |      2      |      6       |
  *-------------*--------------*
  |      3      |      9       |
  *-------------*--------------*

```

When translating from page number `p` to frame number `d`, firstly the cache is checked to see if `p` is in any associative register. If it is, then get `d` out of it. Otherwise, get `d` from the page table in memory.

### Translation Look-aside Buffers (TLBs)

These associative caches are refered to as Translation look-aside buffers. TLBs usually need to be flushed out after a context switch as pages are process specific. This can lead to poor performance / substantial overhead after the context switch as the cache needs to warm up first. To solve this, some TLBs store **address-space ids (ASIDs)** in entries, which are used to uniquely identify each process to provide address-space protection for that process. This allows the TLB to be shared across processes.

Previously, kernel memory was mapped into the virtual address space of each process which is more efficient as these pages didn't have to change in the TLB on a system call. However, more recently kernel memory has been moved to a seperate virtual address space for security reasons. This means that the TLB may have to be changed / flushed on a system call.


## Page Table Types

1. **Hierarchical** page table.
2. **Hashed** page table.
3. **Inverted** page table.

### 1. Hierarchical Page Table

In a traditional page table, the allocated array must be proportional to the size of the virtual address space. This is very waseful as the virtual address space is usually only sparsely populated. **Hierarchical page tables** instead exploit this fact.

#### Two-Level Paging

**Two-level paging** work by using an **outer page table** in conjunction with many inner page tables. The outer page table doesn't map to frame numbers, instead it maps to an address (page address) of an inner page table.

```

   Outer page           Page table              Physical
     table                                       memory
  *----------*        *-------------*          *--------* 0
  |          |\       |  *-------*  |          |  ...   |
  *----------* *------|->|   1   |\ |          *--------* 1
  |          |        |  *-------* *|--------->|########|
  |          |        |  |       |  |          *--------*
  |   ...    |        |  |  ...  |  |          |        |
  |          |        |  |       |  |          |  ...   |
  |          |        |  *-------*  |          |        |
  *----------*        |  |  500  |--|--*       *--------* 100
  |          |-*      |  *-------*  |   \  *-->|########|
  *----------*  \     *-------------*    \/    *--------*
                 \    |     ...     |    /\    |  ...   |
                  \   *-------------*   /  \   *--------* 500
                   \  |  *-------*  |  /    *->|########|
                    *-|->|  100  |--|-*        *--------*
                      |  *-------*  |          |  ...   |
                      |  |       |  |          *--------* 708
                      |  |  ...  |  |     *--->|########|   \
                      |  |       |  |    /     *--------*    \ Frame
                      |  *-------*  |   /      |  ...   |      Number
                      |  |  708  |--|--*       *--------*
                      |  *-------*  |
                      *-------------*

```

Note in this diagram, the each entry in the page table is an inner page table, mapped to by the outer page table.

This means that for any region where there are no allocated pages in virtual memory, there won't be an inner page table allocated.

Remember that a logical address is divided into a page number and page offset. Now that the page table is now paged, the page number is divided up further:

- `p1`: index into the outer page table.
- `p2`: displacement into the page of inner page table.

```

    page number | page offset
  *------*------*-------------*
  |  p1  |  p2  |      d      |
  *------*------*-------------*
     |
     |     *------* 0
     |     |      |
     |     *------* p1
     *---->|######|------>*------* 0
           *------*       |      |
           |      |       *------* p2
           *------*       |######|------>*------* 0
                          *------*       |      |
                          |      |       *------* d
                          *------*       |######|
                                         *------*
                                         |      |
                                         *------*


```

Note that now on a TLB miss, there are now *two* extra lookups instead of just one (this increases with higher levels).

#### Three Level Paging

In some cases, two levels of paging may not be enough. Therefore, there also exist schemes where the level of paging increase to 3 or even 4. The principle remains the same.

```
   2nd outer    Outer      Inner   | Page
    page        page       page    | offset
  *----------*----------*----------*--------*
  |    p1    |    p2    |    p3    |   d    |
  *----------*----------*----------*--------*
```

#### Page Table Size

The size of the page table grows with the size of the virtual address space. Assuming a 4KB page size:

- on a 32-bit machine, the page table will be at least 4MB per process (not too bad).
- on a 64-bit machine, the page table will need 2^52 entries. With 8 bytes per entry, that's 30 million GB per process.

One idea to resolve this is to instead stores entries per *frame* rather than per page. This makes more sense as the page table will grow linearly with the total number of physical frames on the machine. Hashed and inverted page tables do this.

### 2. Hashed Page Table

A **hashed page table** works by using a hash table to to map from virtual addresses to physical addresses.

If there are conflicts in a particular hash bucket, then each entry is chained so they can be searched until the matching virtual page number is found.

This implementation is much more complex. Since this needs to be implemented in hardware (e.g. to traverse the chain in a bucket), the MMU would require a lot more complexity. The effectiveness of this implementation also greatly depends on the quality of the hash function, and how many conflicts there are.

### 3. Inverted Page Table

In an **inverted page table**, the page tables entries consist of:

- the virtual address of the page stored in the memory location.
- information about the process which owns the page.

The index of each entry in the page table is equal to the frame number.

This method decreases memory needed to store the page table, but it also increases the time required to search the table when a page reference occurs (however, this may not be too bad as long as there is a very high hit ratio for the TLB).

## Shared Memory

A process can set up shared memory areas by simply mapping a page from each process' virtual memory to the same physical frame. This means that when either process goes to access that page, they are infact accessing the same memory. This has the benefit that once the memory is shared, there is no need for any kernel involvment, however it is less useful for unidirectional communication due to the lack of synchronisation.

### System V API

- `shmget`: allocates a shared memory segment.
- `shmat`: attaches a shared memory segment to the address space of a process.
- `shmctl`: changes the properties associated with a shared memory segment.
- `shmdt`: detaches a shared memory segment from a process.

## Inter-Process Communication

IPC is an abbreviation that stands for “Inter-process Communication”. It denotes a set of system calls that allows a User Mode process to:
- Synchronize itself with other processes by means of ‘Semaphores’.
- Send messages to other processes or receive messages from them.
- Share a memory area with other processes.


Data sharing among processes can be obtained by storing data in temporary files protected by locks. But this mechanism is never implemented as it proves costly since it requires accesses to the disk filesystem. For that reason, all UNIX Kernels include a set of system calls that supports process communications without interacting with the filesystem.

Application programmers have a variety of needs that call for different communication mechanisms. Some of the basic mechanisms that UNIX systems, GNU/Linux is particular has to offer are:

- Pipes and FIFOs: Mainly used for implementing producer/consumer interactions among processes. Some processes will fill the pipe with data while others will extract from it.

- Semaphores: Here we refer to (NOT the POSIX Realtime Extension Semaphores applied to Linux Kernel Threads), but System V semaphores which apply to User Mode processes. Used for locking critical sections of code.

- Message Queues: To set up a message queue between processes is a way to exchange short blocks (called messages) between two processes in an asynchronous way.

- Shared Memory: A mechanism (specifically a resource) applied when processes need to share large amounts of data in an efficient way.

- A process is a program in execution, and each process has its own address space, which comprises the memory locations that the process is allowed to access. A process has one or more threads of execution, which are sequences of executable instructions: a single-threaded process has just one thread, whereas a multi-threaded process has more than one thread. Threads within a process share various resources, in particular, address space. Accordingly, threads within a process can communicate straightforwardly through shared memory, although some modern languages (e.g., Go) encourage a more disciplined approach such as the use of thread-safe channels. Of interest here is that different processes, by default, do not share memory.

There are various ways to launch processes that then communicate, and two ways dominate in the examples that follow:

- A terminal is used to start one process, and perhaps a different terminal is used to start another.

- The system function fork is called within one process (the parent) to spawn another process (the child).

## Creating Processes

```c
int fork(void)
```

`fork` creates a new child process by making an exact copy of the parent process image. The child process inherits its parent's resources and will begin to execute concurrently with it.

`fork` returns twice, once in the parent, and once in the child:

- **Parent**: return value is the process ID of the child.
- **Child**: return value is `0`.
- **Error**: if fork is unable to create a child, `-1` is returned to the parent. This can happen if there isn't enough memory to create the child process in.

An example of some code using `fork`:

```c
#include <unistd.h>
#include <stdio.h>

int main() {
  if (fork() != 0) {
    printf("Parent process\n");
  } else {
    printf("Child process\n");
  }

  printf("Common code\n");
}
```

## Executing Processes

```c
int execve(
  const char *path,
  char *const argv[],
  char *const envp[]
)
```

`execve` executes a command. Instead of copying the parent process, a completely new process is made.

Its arguments are:

- `path`: the full pathname of the program to be executed.
- `argv`: the arguments passed to this program.
- `envp`: a list of environment variables.

`execve` has many useful wrappers (`execl`, `execle`, `execvp`, `execv`, etc). Read `man execve` for more.

## Waiting for Process Termination

```c
int waitpid(
  int pid,
  int* stat,
  int options
)
```

`waitpid` suspends execution of the calling process until the child process with the PID terminates normally or it receives a signal.

`waitpid` can be used to wait for multiple children:

- `pid = -1`: wait for any child.
- `pid = 0` wait for any child in the same process group as caller.
- `pid = -gid` wait for any child with process group `gid`.

Its return value can be:

- the `pid` of the terminated child process.
- `0` if `WNOHANG` is set in options. This option "is used to indicate that the call should not block if there are no processes that wish to report status" (from `man waitpid`).
- `-1` on error, and `errno` is set to indicate the error (see `man errno`).

## Process Termination

```c
void exit(int status)
```

`exit` terminates the calling process, returning the exit `status` to the parent process (i.e. through `int *stat` in `waitpid`).

```c
void kill(int pid, int sid)
```
`kill` sends signal `sig` to process with PID `pid`.

An example of `exit`:

```c
#include <unistd.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
  int pid = fork();

  if (pid != 0) {
    // parent process
    int *stat;
    waitpid(pid, stat, 0);
    printf("Child exit status is: %d\n", WEXITSTATUS(*stat));
  } else {
    // child process
    exit(4);
  }
}
```

```
$ gcc a.c && ./a.out
Child exit status is: 4
```

## I/O Management

Management of I/O devices is a very important part of the operating system - so important and so varied that entire I/O subsystems are devoted to its operation. ( Consider the range of devices on a modern computer, from mice, keyboards, disk drives, display adapters, USB devices, network connections, audio I/O, printers, special devices for the handicapped, and many special-purpose peripherals.

### Device Controllers

Device drivers are software modules that can be plugged into an OS to handle a particular device. Operating System takes help from device drivers to handle all I/O devices.

The Device Controller works like an interface between a device and a device driver. I/O units (Keyboard, mouse, printer, etc.) typically consist of a mechanical component and an electronic component where electronic component is called the device controller.

There is always a device controller and a device driver for each device to communicate with the Operating Systems. A device controller may be able to handle multiple devices. As an interface its main task is to convert serial bit stream to block of bytes, perform error correction as necessary.

Any device connected to the computer is connected by a plug and socket, and the socket is connected to a device controller. Following is a model for connecting the CPU, memory, controllers, and I/O devices where CPU and device controllers all use a common bus for communication.

### Synchronous vs asynchronous I/O

- Synchronous I/O − In this scheme CPU execution waits while I/O proceeds

- Asynchronous I/O − I/O proceeds concurrently with CPU execution

- Communication to I/O Devices

The CPU must have a way to pass information to and from an I/O device. There are three approaches available to communicate with the CPU and Device.

- Special Instruction I/O
- Memory-mapped I/O
- Direct memory access (DMA)

## Virtualization

Virtualization is the process of running a virtual instance of a computer system in a layer abstracted from the actual hardware. Most commonly, it refers to running multiple operating systems on a computer system simultaneously. To the applications running on top of the virtualized machine, it can appear as if they are on their own dedicated machine, where the operating system, libraries, and other programs are unique to the guest virtualized system and unconnected to the host operating system which sits below it.

There are many reasons why people utilize virtualization in computing. To desktop users, the most common use is to be able to run applications meant for a different operating system without having to switch computers or reboot into a different system. For administrators of servers, virtualization also offers the ability to run different operating systems, but perhaps, more importantly, it offers a way to segment a large system into many smaller parts, allowing the server to be used more efficiently by a number of different users or applications with different needs. It also allows for isolation, keeping programs running inside of a virtual machine safe from the processes taking place in another virtual machine on the same host.

### How does virtualization work?

Software called hypervisors separate the physical resources from the virtual environments—the things that need those resources. Hypervisors can sit on top of an operating system (like on a laptop) or be installed directly onto hardware (like a server), which is how most enterprises virtualize. Hypervisors take your physical resources and divide them up so that virtual environments can use them.

Resources are partitioned as needed from the physical environment to the many virtual environments. Users interact with and run computations within the virtual environment (typically called a guest machine or virtual machine). The virtual machine functions as a single data file. And like any digital file, it can be moved from one computer to another, opened in either one, and be expected to work the same

When the virtual environment is running and a user or program issues an instruction that requires additional resources from the physical environment, the hypervisor relays the request to the physical system and caches the changes—which all happens at close to native speed (particularly if the request is sent through an open source hypervisor based on KVM, the Kernel-based Virtual Machine).


### What is hypervisor?

 A hypervisor is a program for creating and running virtual machines. Hypervisors have traditionally been split into two classes: type one, or "bare metal" hypervisors that run guest virtual machines directly on a system's hardware, essentially behaving as an operating system. Wype two, or "hosted" hypervisors behave more like traditional applications that can be started and stopped like a normal program. In modern systems, this split is less prevalent, particularly with systems like KVM. KVM, short for kernel-based virtual machine, is a part of the Linux kernel that can run virtual machines directly, although you can still use a system running KVM virtual machines as a normal computer itself.

Types of Virtualization:
  - Data virtualization : Data virtualization tools sit in front of multiple data sources and allows them to be treated as single source, delivering the needed data—in the required form—at the right time to any application or user.
  - Desktop Virtualization : desktop virtualization allows a central administrator (or automated administration tool) to deploy simulated desktop environments to hundreds of physical machines at once. 
  - Server Virtualization : Servers are computers designed to process a high volume of specific tasks really well so other computers—like laptops and desktops—can do a variety of other tasks. Virtualizing a server lets it to do more of those specific functions and involves partitioning it so that the components can be used to serve multiple functions.
  - Operating System Virtualization : Operating system virtualization happens at the kernel—the central task managers of operating systems. It’s a useful way to run Linux and Windows environments side-by-side.
  - Network Function Virtualization : Virtualizing networks reduces the number of physical components—like switches, routers, servers, cables, and hubs—that are needed to create multiple, independent networks, and it’s particularly popular in the telecommunications industry.

- Distributed File System Requirements Sharing files across nodes (processes, cpus, computers, networks) requires or benefits from certain attributes of the system.

- Some attributes which are required or may be used as utility metrics for a DFS:

   - naming

   - transparency (works hand-in-hand with naming)

   - concurrency (including synchronization)

   - replication (caching, with consistency checks)

   - platform independence (heterogeneity)

   - fault tolerance

   - consistency

   - security 

## Distributed File Systems

A distributed file system is a file system that resides on different machines, but offers an integrated view of data stored on remote disks

- Distributed file system (DFS) – a distributed implementation of the classical time-sharing model of a file system, where multiple users share files and storage resources.!A DFS manages set of dispersed storage devices! Overall storage space managed by a DFS is composed of different, remotely located, smaller storage spaces. There is usually a correspondence between constituent storage spaces and sets of files. 

  - A DFS manages a set of dispersed storage devices

  - The overall storage space is composed of heterogeneous, remotely located, smaller storage spaces.

  - There is usually a correspondence between constituent storage spaces and sets of files. 

Distributed File System Structure:

  - Service– software entity running on one or more machines and providing a particular type of function to a priori unknown clients.

  - Server– service software running on a single machine.

  - Client–  process that can invoke a service using a set of operations that forms its client interface.

  - A client interface for a file service is formed by a set of primitive file operations(create, delete, read, write).

  - Client interface of a DFS should be transparent, i.e., not distinguish between local and remote files.

- A distributed file system is a client/server-based application that allows clients to access and process data stored on the server as if it were on their own computer. When a user accesses a file on the server, the server sends the user a copy of the file, which is cached on the user’s computer while the data is being processed and is then returned to the server.


## Cloud Computing

Computing  itself,  to  be  considered  fully  virtualized,  must  allow  computers  to be  built  from  distributed components  such  as  processing,  storage,  data,  and software resources.

In the simplest terms, cloud computing means storing and accessing data and programs over the Internet instead of your computer's hard drive. The cloud is just a metaphor for the Internet.

What cloud computing is not about is your hard drive. When you store data on or run programs from the hard drive, that's called local storage and computing. Everything you need is physically close to you, which means accessing your data is fast and easy, for that one computer, or others on the local network. Working off your hard drive is how the computer industry functioned for decades; some would argue it's still superior to cloud computing, for reasons I'll explain shortly.

Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

There are five key characteristics of cloud computing : 

- On-demand self-service : Users can access computing services via the cloud when they need to without interaction from the service provider. The computing services should be fully on-demand so that users have control and agility to meet their evolving needs.

- Broad network access : Cloud computing services are widely available via the network through users’ preferred tools (e.g., laptops, desktops, smartphones, etc.).

- Resource pooling : One of the most attractive elements of cloud computing is the pooling of resources to deliver computing services at scale. Resources, such as storage, memory, processing, and network bandwidth, are pooled and assigned to multiple consumers based on demand.

- Rapid elasticity : Successful resource allocation requires elasticity. Resources must be assigned accurately and quickly with the ability to absorb significant increases and decreases in demand without service interruption or quality degradation.

- Measured service : Following the utility model, cloud computing services are measured and metered. This measurement allows the service provider (and consumer) to track usage and gauge costs according to their demand on resources.

