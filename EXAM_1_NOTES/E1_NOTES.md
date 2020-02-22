## Jan 27 - 31
#### Brief intro of OS
- Operating Systems are:
    - Referees: 
        - Manage shared resources
        - Provide protection and communication for process
    - Illutionists:
        - Provide the illusion of infinite resources
    - Glue: 
        - Provide standard services which the hardware implements

#### History of Operating Systems and Dual Mode Execution
- Phase 1 (Expensive Hardware, Cheap Humans): 
    - 1)One user on the console, one process at a time
        - Problem: Low utilization of expensive components
    - 2)Batch processing: load program, run, output to tape, print results, repeat (1955-1965)
        - Advantage: 
            - Next job can be loaded immediately as previous one finished
        - Disadvantages:
            - No protection - can easily crash bash monitor
            - Computer is idle during I/O (Load program, output to tape, print results)
    - 3)Overlap of I/O and computation, interrupts
        - Performance improves because I/O and processing happen concurrently
        - Concurrency? Concurrency is the execution of several instruction sequences at the same time. In an operating system, this happens when there are several process threads running in parallel. These threads may communicate with each other through either shared memory or message passing. Concurrency results in sharing of resources result in problems like:- deadlocks and resources starvation. Concurrency is the interleaving of processes in time to give the appearance of simultaneous execution.
    - 4)Multiprogramming: several programs run at the same time sharing the machine
        - OS manages interacitons between concurrent programs
        - Requires: memory protection and relocation
        - Keepin several jobsin memory and multiplex CPU between Jobs
- Phase 2 (Cheap Hardware, Expensive Humans): 
    - 5) Interactive Timesharing (1970-)
        - Requires: more sharing, more protection, more concurrency
        - New OS services: shell, file system, rapid process switching, virtual memory
        - New problems: response time, thrashing
    - 6) Personal Computing
        - Computers are cheap
- Phase 3 (Very cheap hardware, very expensie humans):
    - 7) Parallel and distributed computing
        - In parallel systems, multiple processors are in the same machine, sharing memory, I/O devices
        - In distributed systems, multiple processors comminicate via a network
        - Advantages: increases performance, increases reliability, sharing of specialized resources

#### Dual Mode Execution
- OS Interfaces
    - Three interfaces
        - Abstract Machine Interface: between OS and apps: 
        - Application programming interface: function calls provided to applications
        - Hardware abstraction layer: abstracts hardware internally to the OS
- Why is dual mode crucial?
    - If applications had free rein then buggy or malicious applications could:
        - Crash other applications
        - Violate privacy of other applications
        - Hog all the resources
        - Change the OS
        - Crash the os
- Box around the application:
    - An abstraction for protection
        - Represents an application program exevuting with restricted rights
    - Restricting rights must not hinder functionality
- A process is a program during execution. It is the basic unit of execution in an OS. Different processes may run different instances of the same program. At a minimum, process execution requires following resources: Memory to contain the program code and data & a set of CPU registers to support execution
- How can the OS enforce restriced rights?
    - BAD WAY: OS interprets each instruction. TOO SLOW
    - GOOD WAY: Dual mode execution with restricted access in the user mode and unrestricted access in the kernel mode.

#### USER vs KERNEL MODE
    - User MAY NOT/Kernel CAN: 
        - address I/O directly
        - use instructions that manipulate OS memory
        - set the mode bits that determine user or kernel mode
        - disable and enable interrupts
        - halt the machine
- How to transition modes (entering the kernel)(3 Methods)?
    - Exceptions
        - synchronous
    - Interrupts
        - Asynchronous (not related to instruction that just executed)
    - System calls/traps 
        - Synchronous
        - user program requests OS device
- What is happening when user mode to kernel mode?
    - 1) OS saves state of user program
    - 2) Hardware identifies why boundary is crossed (system call?, interrupt?, which exception)
        - Saving the state of the interrupted process
            - Privileged hw register points to exception stack
            - Why not use user-level stack: Reliability (even if user's stack points to invalid address, handlers continure to work), Security (kernel state should not be stored in user space)
            - One exception stack per processor/process/thread
        - System calls: A request by a user-level process to call a function in the kernel is a system call (read(), write(), exit()). It is the interface between the application and the operating system. Parameters passed according to calling convention
            - 1) User process executes a trap instruction
            - 2) Hardware calls the OS at the system-call handler, a prespecified location
            - 3) OS then identifies the required service and parameters, executes the required service, sets a register to contain the result of call, executes an RTI instruction to return to the user program
            - 4) User program recieves the result and continues
    - 3) Hardware selects entry from interrupt vector
    - 4) Appropriate handler is invoked
- When a transition between user mode and kernel mode is required in an operating system, a context switch is not necessary.
    - A Context switch IMPLIES a mode switch
    - Whereas a mode switch DOES NOT imply a context switch
- Context Switch: is the process of switching processes
- Switching back
    - From an interrupt, just reverse all steps because it is asynchronous
    - From exception and system call, increment PC on return
        - Synchronous, so you want to execute the next instruction, not the same one again
        - handler changes PC at the base of the stack
        - on system call, increment is done by the hardware
- For effective protection, the hardware must support at least three features:
    - 1) Privileged instructions
        - prevents user processes from halting the machine
        - mode bit
    - 2) Timer interrupts
        - prevents process from gaining control of the CPU and never releasing it
        - hardware timer
    - 3) Memory protection
        - prevents unauthorized access of data

## Processes and Dual Mode Execution 1-29-20
#### Process
- A process is a program during execution
- A process is the basic unit of executionin an OS
- Different processes may run different instances of the same prgram
- Requires:
    - Memory to contian the program code and data
    - A set of CPU registers to support execution
#### Process State
- Consists of at least:
    - The contents of the address space, including:
        - The code for running the program
        - The static data for running the program
        - An execution stack with the program's call chain
        - The heap, where dynamic data is allocated
    - Registers and their contents, including:
        - The heap pointer
        - The program counter indicating the next instruction
        - The stack pointer 
    - The set of OS resources in use
    - Process identifier
    - Process execution state
#### Process Life Cycle
- new, ready, blocked, running, terminated
#### How does the OS track this data?
- The OS uses a Process Control Block
    - Dynamic kernel data structure kept in memory
    - Represents the execution state and location of each process when it is not executing
- The PCB contains: 
    - Process identification number, program counter, stack pointer, contents of feneral purpose registers, memory management information, username of owner, list of open fiels
- PCBs are initialized when a process is created and deleted when a process terminates
- Only OS can access the PCB
#### How to Create a Process
- One process can create other processes (Parent, Child)
- In some systems, the parent defines resources and privileges to its children
- The parent can either wait for the child to complete or continue in parallel
    - If single core, than parallel is not possible
#### Fork()
- Copies a process into an (identical) process
    - copies variable values and program counter from parent to child
    - Returns twice: once to the parent and once to the child
    - Return value is different in the parent and child
        - Parent: it is the child process id
        - Child: it is 0
    - Both processes begin execution from the same point
    - Each process has its own memory and its own copy of each variable
#### Exec()
- Overlays a process with a new program
    - PID does not change
    - Arguments to new program may be specified
    - Code, stack, and heap are overwritten
- Child processes often call exec() to start a new and different program
- If call is successful, it si the same process, but it is running a different program
#### The wait() system call
- Causes the parent process to wait for the child process to terminate
    - Allows parent process to get return value from the child
    - When a child calls exit(), the OS unblocks the parent and returns the value passed by exit() as a result of the wait call
    - If there are no children alive, wait() returns immediately
    - Also, if there are zombies waiting for thei parents, wait() returns one of the values immediately
- The parent will block for the child to finish
#### Zombie Process
- Process has terminated
- Parent process has not collected its status
#### Terminating a process exit()
- Takes the result of the process as an argument
#### Terminating a process kill()
- A parent can terminate a child using kill
- Sends a signal to a specified process
- There are handlers, but it one does not exist, the default action is taken
#### Orphaned Processes
- Parent terminates before the child
- Child can orphan itself to keep running in the background
#### Process Control
- OS must include calls to enable special control of a process
    - Priority manipulation
    - Debugging support
    - Alarms and time

## CPU Scheduling 2-3-20
- The OS uses multiprogramming, or concurrency, to increase system utilization and throughput by overlapping I/O and CPU activities 
- Long-Term Scheduling
    - How does the OS determine the degree of multiprogramming the number of jobs residing at once in the primary memory?
- Short-Term Scheduling
    - How does the OS select a process from the ready queue to execute?
#### Short-Term Scheduling
- The scheduler schedules the next process on the CPU
    - It is the mechanism
    - It implements a policy
- The scheduler (maybe) executes when" 
    - A process switches from running to blocking
    - A process is created or terminated
    - An interrupt occurs
        - I/O devie finished
        - Periodic timer
#### Scheduling Policies: Two Big Categories
- Scheduling algorithms are divided into categories based on their use of the timer interrupt
    - Non-preemptive
        - An executing process is not ever pre-empted
        - Scheduler runs when process blocks or terminates -- not on hardware interrupts
    - Preemptive
        - An executing process might be pre-empted from the CPU
        - Scheduler runs on interrupts, mostly timer, but also system calls
        - Most OSes today have this
#### Evaluating Scheduling Policies
- CPU Utilization: Percentage of time that the CPU is busy
- Throughput: Number of processes completing in a unit of time
- Turnaround Time: Length of time to run a process from initialization to termination, including all the waiting time
- Response Time: Time between issuing a command and getting a result (User Interaction/Feedback)
- Waiting Time: Total amount of time that a process is in the ready queue
#### Scheduling Policies
- Ideal CPU scheduler
    - Maximizes CPU utilization and throughput
    - Minimizes turnaround time, waiting time, and response time
    - Requires knowledge of process behavior we typically don't have
#### Scheduling Algorithms
- One process per user
- One thread per process
    - More on this topic next time
- Processes are independent (Don't worry about waiting for child processes)
- First Come First Served (FCFS)
    - Also known as First In First Out (FIFO)
    - Scheduler executes jobs in arrival order
    - Jobs run until they either complete or block on I/O
    - In early FCFS schedulers, the job did not relingquish the CPU eve when it was doing I/O
    - ADVANTAGE(S):
        - Simple to implement
        - Intuitively fair
    - DISADVANTAGE(S):
        - Not preemptive -> not suitable for interactive jobs
        - Long-running process will delay all jobs behind it
    
- Round Robin 
    - (Time Slice - quantum)
    - Add a timer and use a pre-emptive policy
    - Run each process for its time slice (Scheduling quantum)
    - After each time slice, move the running process to the back of the queue
    - Selecting a time slice: 
        - Too large: waiting time suffers, degenerates to FCFS if processes are never preempted
        - Too small: Throughput suffers because too much time is spent context switching
        - Balance the two by selecting a time slice where context switching is roughly 1% if the time slice
    - Most time sharing systems use some variation of this policy
    - A typical time slice today is between 10-100 miliseconds, with a context siwtch time of 0.1 to 1 milisecond
    - ADVANTAGE(S):
        - Fair that every proess gets an equal share of the CPU.
        - If we know the number of processes on the run queue, we can know the worst-case response time for a process
    - DISADVANTAGE(S):
        - Giving every process an equal share of the CPU is not always a good idea. (highly interactive processes will get scheduled no more frequently than CPU-bound processes)
- Shortest Job First SJF
    - Schedule the job that has the least amount of work (measured in CPU time) to do until its next I/O request or termination
        - I/O bound jobs get priority over CPU bound jobs
    - Works for preemptive and non-preemptive schedulers
    - DISADVANTAGE(S):
        - Requires Approximations
        - Might never get to a large job (starvation)
- *** Using the Past to Predict the Future ***
    - If a process isI/O bound in the past, it is also likely to be I/O bound in the future
    - To exploit this behabior, the scheduler can fabor jobs when they use very little CPU time, thus approximating SJF
    - This policy is adaptive because it relies on past behavior and changes in behabior result in changes to scheduling decisions
- Approximating SJF: Multilevel Feedback Queues
    - Multiple queues with different priorities
    - OS uses Round Robin scheduling at each priority level, running the jons in the highest priority queue first
    - Once those finish, OS runs jobs out of the next highest priority queue
        - Can lead to starbation if highest priority jobs run forever
    - Round robin time slice increases exponentially at lower priorities

    - Adjust priorities as follows
        1. Job starts in the highest priority queue
        2. If job's time slice expires, drop its priority one level, but no further tha the lowest level
        3. If job's time slice does not expire, then increase its priority one level, up to the top priority level
#### Improving Fairness
- Since SJF is optimal, but unfair, any increase in fairness by giving long jobs a fraction of the CPU when shorter jobs are available will degrade average waiting time
    - Give each queue a fraction of the CPU time
    - Adjust the priority of jobs as they do not get serviced