# Dicussion Sections for Exam 1
### Discussion Section 4: 
### Compare and contrast monitors and semaphores. What is an appropriate use of each?
- Similarities
    - Both allow processes to access the shared resources in mutual exclusion
    - Both are the process synchronization tool
- Differences
    - Action
        - Semaphore
            - The value of Semaphore S indicates the number of shared resources available in the system
        - Monitor
            - The monitor type contains shared variables and the set of procedures that operate on the shared variable
    - Access
        - Semaphore
            - When any process access the shared resources it performs wait() operation on S and when it releases the shared resouces it performs signal() operation on s.
        - Monitor
            - When any process wants to access the shared variables in the monitor, it needs to access it through the procedures
    - Condition Variables
        - Semaphore 
            - None
        - Monitor
            - Does

- Definitions
    - Monitor: 
        - To overcome the timing errors that occurs while using semaphore for process synchronization, Monitors are utilized instead
        - Mainly used in user level rather than semaphores as they are safer but more risky in casuing timing errors. 
        - Abstract data types and contain shared data variables and procedures. The shared data variables cannot be directly accessed by a process and procedures are required to allow a single process to access the shared data variables at a time. 
        - Only one process can be active in a monitor at a time. Other processes that need to access the shared variables in a monitor hae to line up in a queue and are only provided access when the previous process release the shared variables.
    - Semaphores
        - Signalling mechanism and a thread that is waiting on a semaphore can be signalled by another thread. This is different from a mutex as the mutex can be signalled only by the thread that called the wait function (MUTEX == LOCKS). 
        - Uses two atomic operations, wait and signal for process synchronization
        - Counting and binary semaphores