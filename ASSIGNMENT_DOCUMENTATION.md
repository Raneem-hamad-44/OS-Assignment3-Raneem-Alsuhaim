# Assignment 3 - Complete Documentation

**Student Name**: [Raneem hamad]  
**Student ID**: [ 444051957]  
**Date Submitted**: [ 30-4-2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 
Entry 1 - April 29, 2026, 5:30 PM

What I implemented:
I implemented the core structure of the CPU Scheduler simulation using Round Robin algorithm. I created the Process class with attributes such as burst time, remaining time, priority, and time quantum. I also implemented the ready queue using Queue<Thread> and mapped each thread to its corresponding process.

Challenges encountered:
I faced difficulty in managing the process lifecycle, especially how to reinsert processes back into the queue after executing a time quantum while keeping track of remaining time.

How I solved it:
I solved this by updating the remainingTime after each execution and checking if the process is finished using isFinished(). If not finished, I re-added it to the queue using the addProcessToQueue method.

Testing approach:
I tested the scheduler by running multiple processes with random burst times and verifying that each process gets CPU time in a cyclic order (Round Robin behavior).

Time spent: 2 hours
---

### Entry 2 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

Entry 2 - April 29, 2026, 8:00 PM

What I implemented:
I added synchronization mechanisms to protect shared resources. Specifically, I implemented multiple ReentrantLock objects to protect variables such as contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog.

Challenges encountered:
The main challenge was understanding where race conditions might occur and ensuring that shared variables are accessed safely across multiple threads.

How I solved it:
I wrapped critical sections using lock() and unlock() inside try-finally blocks to ensure thread safety. I also used separate locks for different shared resources to improve concurrency.

Testing approach:
I ran the program multiple times and monitored the counters and logs to ensure there were no inconsistent values or unexpected behavior due to concurrent access.

Time spent: 1 hour
---

### Entry 3 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

Entry 3 - April 30, 2026, 4:00 PM

What I implemented:
I implemented the Semaphore to simulate CPU access control, allowing only one process to execute at a time. I also added execution logging, waiting time calculation, and final statistics printing.

Challenges encountered:
I had difficulty integrating the semaphore with thread execution without causing deadlocks or blocking issues.

How I solved it:
I used cpuSemaphore.acquire() before execution and release() inside a finally block to guarantee that the CPU is always released. I also carefully structured the thread execution using start() and join().

Testing approach:
I tested edge cases such as the last process running to completion and verified that statistics like average waiting time and completed processes are calculated correctly.

Time spent: 1 hour
---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Your answer here - 4-6 sentences with code examples]:


One race condition occurs with the shared variable `contextSwitchCount`. Multiple threads may try to increment this variable at the same time using `contextSwitchCount++`, which is not an atomic operation. This can lead to lost updates where some increments are overwritten, resulting in an incorrect total count of context switches.

Another race condition occurs with the shared `executionLog` list. Since `ArrayList` is not thread-safe, concurrent access from multiple threads adding log messages (e.g., `executionLog.add(message)`) can corrupt the internal structure of the list or cause inconsistent log entries. This may result in missing or duplicated log messages during execution.

Both cases show that without synchronization, concurrent access to shared resources can lead to incorrect program behavior and unreliable results.

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Your answer here - explain your implementation choices]


ReentrantLock is used for mutual exclusion, meaning it allows only one thread to access a critical section at a time. It is mainly used to protect shared variables from race conditions. In my code, I used ReentrantLock to protect shared resources such as contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog to ensure that updates to these variables are done safely without interference from other threads.
On the other hand, a Semaphore is used to control access to a limited number of resources. It allows a specific number of threads to access a resource simultaneously based on the number of permits. In my code, I used a Semaphore (cpuSemaphore) with one permit to simulate a single CPU, ensuring that only one process executes at a time.
I used ReentrantLock for data protection (critical sections), while I used Semaphore for resource management (CPU access control).
---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Your answer here - reference try-finally blocks, lock ordering, etc.]

Deadlock is a situation where two or more threads are blocked forever because each thread is waiting for a resource held by another thread. This results in the program freezing, as none of the threads can proceed.
One prevention technique is using try-finally blocks to ensure that locks are always released. In my code, I used lock() and unlock() inside try-finally blocks (e.g., in incrementContextSwitch()), which guarantees that the lock is released even if an exception occurs, preventing threads from being permanently blocked.
Another technique is avoiding nested locks or maintaining a consistent lock ordering. In my implementation, I used separate locks for each shared resource and avoided acquiring multiple locks at the same time. This eliminates circular waiting conditions, which is one of the main causes of deadlock.
Additionally, I used a Semaphore with proper acquire() and release() inside a finally block to ensure the CPU resource is always released, further preventing deadlock situations.
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]


In my implementation, I used separate locks for each counter (contextSwitchLock, completedProcessLock, and waitingTimeLock), which represents a fine-grained locking approach. I chose this design because each counter is independent and does not depend on the others, so they do not need to be synchronized together under one lock.
The advantage of fine-grained locking is that it improves concurrency, since multiple threads can update different counters at the same time without blocking each other. For example, one thread can increment contextSwitchCount while another thread updates totalWaitingTime simultaneously.
The trade-off is that fine-grained locking is slightly more complex to implement and requires careful management of multiple locks, which can increase the chance of mistakes if not handled properly.
In contrast, a coarse-grained lock (one lock for all counters) would be simpler but would reduce performance because all counter updates would be serialized even if they are unrelated. Therefore, since the counters are independent, the fine-grained approach provides better efficiency and scalability in this case.
---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

Which variables:
The variables are contextSwitchCount, completedProcessCount, and totalWaitingTime.

Why they need protection:
These variables are shared among multiple threads, and each thread may try to update them simultaneously. Operations like incrementing (++) and addition (+=) are not atomic, so concurrent access can lead to race conditions, lost updates, and incorrect final results.

Synchronization mechanism used:
I used ReentrantLock (fine-grained locking), with a separate lock for each variable to ensure thread-safe updates while allowing better concurrency.

Code snippet:

public static void incrementContextSwitch() {
    contextSwitchLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        contextSwitchLock.unlock();
    }
}

public static void incrementCompletedProcess() {
    completedProcessLock.lock();
    try {
        completedProcessCount++;
    } finally {
        completedProcessLock.unlock();
    }
}

public static void addWaitingTime(long time) {
    waitingTimeLock.lock();
    try {
        totalWaitingTime += time;
    } finally {
        waitingTimeLock.unlock();
    }
}

Justification:
These counters are critical shared resources that must remain consistent across all threads. Using ReentrantLock ensures mutual exclusion, preventing race conditions during updates. By using separate locks for each counter, the program also maintains higher concurrency, allowing different counters to be updated simultaneously without unnecessary blocking.
---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

What resource:
The shared resource is executionLog (an ArrayList<String>).

Why it needs protection:
The executionLog is accessed by multiple threads simultaneously to store execution messages. Since ArrayList is not thread-safe, concurrent additions can cause data corruption, missing entries, or inconsistent internal state of the list. This can lead to unreliable logging and incorrect simulation output.

Synchronization mechanism used:
I used a ReentrantLock (logLock) to ensure that only one thread can modify the executionLog at a time.

Code snippet:

public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}

Justification:
Using ReentrantLock ensures mutual exclusion when updating the execution log, preventing race conditions and maintaining data integrity. This guarantees that all process execution messages are recorded correctly and in a consistent order. Without this protection, concurrent writes could corrupt the list or cause missing log entries, which would affect debugging and system analysis.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

Purpose of semaphore:
The semaphore is used to control access to the CPU, ensuring that only a limited number of processes can execute at the same time. In this simulation, it is used to model a single CPU core where only one process can run at a time.

Number of permits and why:
The semaphore is initialized with 1 permit (new Semaphore(1)), because the system is simulating a single CPU. This guarantees that only one thread (process) can enter the critical execution section at any given time.

Where implemented:
It is implemented inside the run() method of the Process class, where each process must acquire the semaphore before executing and release it after finishing or leaving the critical section.

Code snippet:

public void run() {
    try {
        SharedResources.cpuSemaphore.acquire();
        try {
            // CPU execution section
            Thread.sleep(runTime);
        } finally {
            SharedResources.cpuSemaphore.release();
        }
    } catch (InterruptedException e) {
        System.out.println("Process interrupted");
    }
}

Effect on program behavior:
The semaphore ensures mutual exclusion at the CPU level, meaning only one process executes at a time even though multiple threads exist. This prevents concurrent execution conflicts and simulates real CPU scheduling behavior. It also helps enforce controlled execution flow and prevents race conditions in the execution phase of processes.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

What I tested: Running program multiple times to verify consistent results

Testing procedure:

javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync

Results:
After running the program multiple times (at least 5 executions), the results were consistent in terms of:

Total number of completed processes matched the expected number each run
Context switch count remained logically consistent with scheduling behavior
No missing or duplicated log entries in the execution log
Average waiting time changed only due to random burst time generation, not due to errors

This shows that the program behaves reliably under repeated execution and maintains correct synchronization of shared data.

Why synchronization is necessary:
Synchronization is necessary because multiple threads access and modify shared resources such as contextSwitchCount, completedProcessCount, totalWaitingTime, and executionLog. Without synchronization, race conditions could occur where multiple threads update these variables at the same time, leading to lost updates (e.g., incorrect increments), corrupted log data, or inconsistent statistical results. For example, two threads could simultaneously execute contextSwitchCount++, causing one update to be overwritten. Similarly, concurrent access to ArrayList could corrupt the execution log structure.

Therefore, locks and semaphores are required to ensure mutual exclusion and maintain data integrity across all threads.

Conclusion:
The testing confirms that the synchronization mechanisms (ReentrantLock and Semaphore) successfully prevent race conditions and ensure consistent and reliable program execution across multiple runs.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 


Test 2: Exception Testing

What I tested:
I tested whether the program produces a ConcurrentModificationException when multiple threads access and modify shared data structures, especially the executionLog (ArrayList) and other shared counters.

Testing procedure:
To test this, I repeatedly ran the scheduler simulation while processes were executing and logging messages concurrently. I also monitored the program output and terminal behavior during multiple executions of processes in the ready queue.

Results:
No ConcurrentModificationException occurred during any of the runs. The program executed successfully each time, and all processes completed without runtime errors. The execution log remained consistent, and no crashes or corrupted outputs were observed.

What this proves:
This proves that the synchronization mechanisms implemented in the program (especially the use of ReentrantLock for the executionLog and shared counters) are working correctly. Without proper synchronization, concurrent modifications of ArrayList would likely cause ConcurrentModificationException or data corruption. The absence of such errors confirms that mutual exclusion is properly enforced and shared resources are safely protected across multiple threads.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 


What I tested:
I tested whether the final output values of the scheduler are correct, including total burst time handling, number of context switches, and completed processes. I verified that all processes complete execution correctly and that the scheduler follows Round Robin behavior.

Expected values:

Total completed processes = number of generated processes (numProcesses)
Context switches = depends on number of processes and time quantum (should increase with more switching between processes)
Total waiting time = sum of waiting times for all processes
No process should remain unfinished at the end of execution

Actual values:

Completed processes matched exactly the generated numProcesses value
Context switch count increased dynamically based on scheduling cycles and was consistent with expected Round Robin execution
Total waiting time was accumulated correctly using addWaitingTime()
All processes reached completion either through normal quantum execution or runToCompletion() for the last process

Analysis:
The results confirm that the scheduler logic is correct and follows the expected Round Robin algorithm behavior. The context switching mechanism is functioning properly, and all processes are guaranteed to finish execution. The synchronization of shared counters ensures that final statistics (context switches, completed processes, and waiting time) are accurate and free from race conditions. This verifies both the correctness of the scheduling algorithm and the effectiveness of the synchronization design.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

I tested different configurations of the scheduler, including increasing the number of processes and changing the time quantum value to observe how the system behaves under different loads.

Purpose:
The purpose was to evaluate the flexibility and correctness of the Round Robin scheduler under different conditions and to understand how changes in time quantum and process count affect performance, context switching, and waiting time.

Results:
When the number of processes was increased, the ready queue became longer and the number of context switches increased significantly. When using a smaller time quantum, processes were switched more frequently, leading to higher context switching overhead but more responsive scheduling. When using a larger time quantum, fewer context switches occurred, but processes took longer to complete individually.

What I learned:
I learned that the time quantum has a direct impact on system performance and efficiency. A small quantum improves responsiveness but increases overhead due to frequent context switching, while a large quantum reduces overhead but may delay process execution. I also learned that increasing the number of processes increases system load and makes synchronization more critical to maintain correct execution and statistics.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

Through this project, I learned that synchronization is essential when multiple threads access shared resources at the same time. Without proper protection, race conditions can easily occur and lead to incorrect results such as lost updates or corrupted data structures. I understood the difference between using ReentrantLock for mutual exclusion and Semaphore for controlling access to limited resources like the CPU. I also learned that choosing the correct synchronization method depends on the type of resource being protected. One important insight was that even simple operations like ++ are not thread-safe and must be protected in concurrent environments. Additionally, I realized that improper synchronization can cause subtle bugs that are difficult to detect because they may not appear every run. Overall, this project improved my understanding of thread safety, concurrency control, and how operating systems manage process scheduling in a real-world-like simulation.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

Example 1:
Banking systems (ATM transactions and online banking). Synchronization is critical when multiple users access the same account at the same time. Without proper locking, two transactions could withdraw money simultaneously, leading to incorrect balances or double spending. Banks use synchronization mechanisms to ensure that each transaction is processed safely and consistently.

Example 2:
Operating systems process scheduling and CPU management. In multi-process systems, multiple processes compete for CPU time and shared resources such as memory or I/O devices. Synchronization ensures that only one process accesses critical resources at a time (or a controlled number using semaphores), preventing conflicts, data corruption, and system instability.

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

I would explain synchronization as a way to make sure multiple threads (or tasks) don’t interfere with each other when using shared resources. A simple analogy is a bathroom in a house: even if many people want to use it, only one person can enter at a time, and the door lock ensures order. In programming, shared resources like variables or lists work the same way—they must be “locked” so only one thread can use them at a time.
Without synchronization, threads can overlap their work and cause mistakes, like two people updating the same number at the same time and one update getting lost. This is called a race condition. Synchronization tools like locks and semaphores help control access so everything happens in a safe and organized way.
In my scheduler program, I used locks to protect shared counters and logs, and a semaphore to control CPU access. This ensures that even though many processes exist, they run without breaking or corrupting shared data.

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
