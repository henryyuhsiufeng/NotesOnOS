## Exam Review
#### Intro
- Name examples of the oS being the Glue, Illutionist, and Referee
    - CPU Scheduling
    - Glue: Keyboard and keyboard drivers
    - Referee: Dual mode execution, setting priviledges
- Name a syscall that interacts with the API, AMI, and HAL
    - write() 
    - All syscalls interact with the API and AMI
    - Read and write for HAL

- History
    - Batch processing
    - Overlapping I/O
        - No wasting time while I/O is executing its stuff
        - Still on the batch system
    - Multiprogramming
        - Interactive time sharing, multiple programs running at once
    - Time sharing
        - Giving a certain amount of time for each process to run on the CPU

- Process
    - What main resources does it need?
        - Address space
        - code
        - register
        - pid
        - 
    - Stack
        - The process Stack contains the temporary data such as method/function parameters, return address and local variables.	
    - Heap
        - This is dynamically allocated memory to a process during its run time.
    - Text
        - This includes the current activity represented by the value of Program Counter and the contents of the processor's registers.
    - Data
        - This section contains the global and static variables.

- Interrupt vector is called whenever switched to kernel level
- When transitioning from user to kernel mode, are you a user or kernel thread?
    - Both
    - Both can enter kernel mode
    - Main distinction is that kernel thread is known by the OS
    - Nothing to do with dual mode execution

- What's the difference between nonpreemptive and preemtive schedulers?
- Which scheduling policies are preemptive?
- Be able to make pro and con list for all schedulers and their evaluations
    - Fifo PRO:high throughput, turnaround , CON: Waiting time, response time

- How does mlfq prevents starvations, how?
    - Implements approximation of shortest job first

- Process vs Thread Lifecycle?
    - no
    - know how to draw it

- Explain how threads share a process's address space
    - share variables as a means to communicate

- Fork() vs creae_thread()
    - threads will have the same process id but their own tid

- Pros/cons of user and kernel thread
    - kernel threads are more costly when switching 
    - kernel can use multicore more better
    - user less costly
    - os knows about kernel level threads

- Locks and semaphores
    - safety
    - liveness
    - bounded waiting
    - failure atomicity

- How do binary and counted semaphores differ? wjem ,ight you use each type?
- Conditions for deadlock and how to break them?
- Compare locks, sempahores, and monitors
- Name the six commandments of writing multi threaded user level code
    - follow same pattern
    - never sleep
    - acquire lock at beginning and end
    - wait in while loop
    - synchronize with locks and condition variables

- name the difference between mesa/hansem and hoare semantics