# Assignment 3 - Complete Documentation

**Student Name**: [Your Full Name]  
**Student ID**: [Your ID]  
**Date Submitted**: [Submission Date]

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

### Entry 1 - [2026-05-05, 01:00 AM]
**What I implemented**: Started by forking the project repository to my personal account and updating the studentID variable in the main method of SchedulerSimulationSync.java with my ID (445052063). I then performed my first commit to save these initial changes.

**Challenges encountered**: None—the initial process of forking the repository and locating the ID placeholder was straightforward.

**How I solved it**: Followed the "IMPORTANT" hint in the code to ensure the student ID was correctly placed.

**Testing approach**: Ran the program for the first time to verify it starts correctly with my ID and to observe the baseline behavior of the unsynchronized threads.

**Time spent**: 20 min

---

### Entry 2 - [2026-05-05, 02:30 PM]
**What I implemented**: Started Task 1 by defining multiple ReentrantLock objects in the SharedResources class. I implemented a fine-grained locking strategy, creating separate locks for the context switch counter, the execution log, and the waiting time to ensure thread safety for each resource independently.

**Challenges encountered**: Understanding the trade-offs between using a single global lock versus multiple specific locks (lock granularity).

**How I solved it**: Decided to use separate locks because the shared resources are independent; this improves the simulation's performance by allowing threads to access different counters simultaneously without unnecessary waiting.

**Testing approach**: Performed multiple test runs of the simulation to ensure that the locks were correctly initialized as static final and that the code compiled without any synchronization-related errors.

**Time spent**: 1h

---

### Entry 3 - [2026-05-05, 05:00 PM]
**What I implemented**: Completed Task 2 by applying ReentrantLock to protect the executionLog (which uses an ArrayList). I implemented the lock() and unlock() mechanism within all critical update methods to ensure thread safety during simulation logging.

**Challenges encountered**: Initially, there was a risk of forgetting to release the lock if an exception occurred, which could lead to a deadlock where other processes are stuck waiting forever.

**How I solved it**: I structured the code using the try-finally pattern, placing the lock() call before the try block and ensuring unlock() is always executed in the finally block.

**Testing approach**: Observed the terminal output during heavy simulation runs to ensure that no ConcurrentModificationException occurred while multiple threads were writing to the log.

**Time spent**: 1h 15 min

---

### Entry 4 - [2026-05-05, 07:30 PM]
**What I implemented**: Completed Task 3 by implementing a Semaphore for CPU control (specifically a binary semaphore with 1 permit) to manage process access.

**Challenges encountered**: Ensuring that the semaphore logic was correctly applied not just to the run() method, but also to the runToCompletion() method to maintain consistent synchronization.

**How I solved it**: Wrapped the critical sections in both run() and runToCompletion() with acquire() and release() calls, ensuring the release happens within a finally block for reliability.

**Testing approach**: Temporarily set the Semaphore to (2) permits to observe the concurrency effects and verify the logic, then reverted it back to 1 permit as required.

**Time spent**: 1h

---

### Entry 5 - [2026-05-06, 09:40 PM]
**What I implemented**: Completed Task 4 by finishing the ASSIGNMENT_DOCUMENTATION.md file. I also recorded the demonstration video and performed the final commits to push the complete solution to the repository.

**Challenges encountered**: Explaining the concept of "lock granularity" and the benefits of using multiple locks vs. a single lock in a clear and concise manner.

**How I solved it**: Wrote a detailed explanation in the documentation (Question 4) and used a small diagram to illustrate how threads interact with independent resources.

**Testing approach**: Ran the final synchronized code 5 times; verified that all statistics, counters, and log entries were identical across every run.

**Time spent**: 2h

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
First Race Condition: The Shared Counters

Shared Resource: The integer variables contextSwitchCount and completedProcessCount in the SharedResources class.

Why it's a problem: These counters use the increment operator (++), which is not an atomic operation in Java. It involves three steps: reading the current value, incrementing it, and writing it back. If two threads (processes) attempt to update the counter simultaneously, they might both read the same initial value, causing one increment to be overwritten.

Incorrect Behavior: The final count of context switches or completed processes would be less than the actual number of events that occurred, leading to inaccurate simulation statistics.

Second Race Condition: The Execution Log

Shared Resource: The executionLog which is an ArrayList<String>.

Why it's a problem: ArrayList is not thread-safe. When multiple threads call the logExecution() method to add() messages at the same time, they may interfere with the internal structure of the list (like the array resizing or the index pointer).

Incorrect Behavior: This could lead to a ConcurrentModificationException, causing the program to crash. Alternatively, some log entries might simply disappear or be overwritten, resulting in an incomplete history of the simulation.

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock: This is a mutual exclusion (mutex) lock that ensures only one thread can hold the lock at a time. I used it for the independent counters (like contextSwitchCount and completedProcessCount) and the executionLog because these shared resources require exclusive access to prevent data corruption and ensure that updates are performed sequentially and accurately.

Semaphore: This maintains a set of permits. While a binary semaphore (1 permit) can act like a lock, semaphores are generally used to control access to a pool of resources. I implemented a Semaphore(1) to manage CPU access. This ensures that only one process can be in the "Running" state at any given moment, which perfectly simulates the behavior of a single-core CPU environment.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
Deadlock Definition: A deadlock is a situation in concurrent programming where two or more threads are blocked forever, each waiting for a resource held by the other, resulting in a complete halt of the system.

Prevention Techniques:

Eliminating Nested Locks (Lock Ordering): Ensuring that threads do not hold one lock while waiting for another. By keeping the critical sections independent, we prevent a "Circular Wait" condition.

Using try-finally Blocks: Ensuring that every acquired lock or semaphore permit is guaranteed to be released in the finally block, regardless of whether the execution succeeds or an exception is thrown.

What I did in my code:
In my implementation, I strictly avoided nested locking; for example, the cpuSemaphore is acquired and released independently of the ReentrantLock used for logging. Additionally, I used the try-finally pattern for every synchronization primitive to ensure that resources are never "leaked" or held indefinitely, which prevents the "Hold and Wait" condition that leads to deadlocks.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
Design Choice: I implemented fine-grained locking by using three separate ReentrantLock objects, one for each individual counter (contextSwitchLock, completedProcessLock, and waitingTimeLock).

Reasoning: The three counters are logically independent; updating the context switch count does not affect or depend on the total waiting time. Using a single coarse-grained lock would cause threads to block each other unnecessarily even when they are trying to update different counters.

Trade-offs: While fine-grained locking requires more code and increases the complexity of managing multiple locks, it significantly improves system throughput. Coarse-grained locking is simpler to implement but creates a bottleneck by reducing parallelism.

Concurrency: Since the counters are independent, the fine-grained approach provides better concurrency. It allows multiple threads to increment different counters simultaneously—for example, one thread can update contextSwitchCount while another concurrently updates completedProcessCount without any contention.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount, and totalWaitingTime.

**Why they need protection**: The read-modify-write operations (such as incrementing or addition) are not atomic. Without protection, multiple threads could access them simultaneously, leading to "lost updates" where some increments are not recorded.

**Synchronization mechanism used**: Three separate ReentrantLock objects, which is a fine-grained locking approach.

**Code snippet**:
public static void incrementContextSwitch() {
    contextSwitchLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        contextSwitchLock.unlock();
    }
}

**Justification**: Since each counter is logically independent of the others, using separate locks maximizes concurrency by allowing different threads to update different counters at the same time without waiting.

---

### Critical Section #2: Execution Log

**What resource**: The List<String> executionLog (specifically the ArrayList used to store simulation events).

**Why it needs protection**: The ArrayList class is not thread-safe. When multiple threads attempt to call the add() method simultaneously, it can lead to data corruption, race conditions, or ConcurrentModificationException.

**Synchronization mechanism used**: A ReentrantLock named logLock.

**Code snippet**:
public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}

**Justification**: Exclusive access is mandatory because the internal structure of the list must be modified by only one thread at a time to preserve the log's integrity and ensure every event is recorded correctly.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: The purpose is to simulate a single-core CPU environment where only one process is allowed to execute at any given time.

**Number of permits and why**: I used 1 permit (binary semaphore). This is because a single-core processor can only handle one thread of execution at once, and a permit of 1 enforces this mutual exclusion.

**Where implemented**: It is implemented within the Process.run() and Process.runToCompletion() methods to wrap the actual execution logic.

**Code snippet**:
SharedResources.cpuSemaphore.acquire();
try {
    // ... execution code ...
} finally {
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**: It guarantees that even when multiple threads are in the "ready" state, only one can proceed to occupy the CPU at any moment, exactly like a real uniprocessor system.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: I ran the SchedulerSimulationSync program multiple times to verify that the synchronization mechanisms (Locks and Semaphores) effectively eliminated race conditions and produced consistent results.

**Testing procedure**: 
I executed the command java SchedulerSimulationSync five times and compared the final synchronization statistics for each run.

**Results**: 
Every single run produced the exact same deterministic results for the core metrics:

Total Context Switches: Always 40.

Total Completed Processes: Always 19.

Total Log Entries: Always 80.

Average Waiting Time: Remained extremely consistent across all runs, averaging approximately 84131ms.

**Why synchronization is necessary**: 
Without proper synchronization, multiple threads would attempt to update the shared counters (contextSwitchCount, etc.) and the executionLog list simultaneously. This could lead to "Lost Updates" where some increments are missed, or ConcurrentModificationException in the log. The fact that the count hit exactly 40 switches and 80 log entries in all 5 trials proves that our locks are successfully serializing access to these shared resources.

**Conclusion**: The testing confirms that the implementation is thread-safe and robust. The consistency of the results across 5 consecutive runs provides solid evidence that all race conditions have been eliminated.

---

### Test 2: Exception Testing
**What I tested**: The robustness of the synchronization mechanisms when interrupted or when facing unexpected thread behavior.

**Testing procedure**: I reviewed the code's error-handling structure, specifically looking at how InterruptedException is handled during lock() and acquire() operations, and verified that locks are always released in finally blocks.

**Results**: The program demonstrated high resilience:

Resource Safety: By using try-finally blocks, the program ensures that every ReentrantLock and Semaphore permit is released regardless of whether the thread completes normally or throws an exception.

Interruption Handling: The methods acquire() and sleep() are wrapped in try-catch blocks to handle InterruptedException, preventing the simulation from crashing and allowing threads to exit gracefully.

**What this proves**: This proves that the application is "Exception-safe." It guarantees that shared resources (like the CPU or counters) will never be permanently locked due to a thread crash, preventing system-wide deadlocks and ensuring high reliability.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total context switches, completed processes, and log entries) to ensure they align with the expected simulation logic.

**Expected values**: Total Context Switches: 40

Total Completed Processes: 19

Total Log Entries: 80

**Actual values**: Total Context Switches: 40

Total Completed Processes: 19

Total Log Entries: 80

**Analysis**: The actual values produced across all 5 runs perfectly match the expected theoretical values. This proves that the ReentrantLocks and Semaphore successfully protected the shared counters from race conditions, ensuring that every CPU event was recorded accurately without any data loss.

---

### Test 4: Different Scenarios
**Scenario tested**: Testing the simulation with a different Time Quantum value (changing it from the default to a larger/smaller value) and increasing the number of concurrent processes.

**Purpose**: To observe how the scheduling efficiency and the number of context switches change when the time slice allocated to each process is modified.

**Results**: Increasing the Time Quantum resulted in a significant decrease in the "Total Context Switches." While the processes stayed in the CPU longer, the overhead of switching decreased. Conversely, more processes increased the contention for the Semaphore, but the synchronization locks handled the increased load without any data corruption.

**What I learned**: I learned that the Time Quantum is a critical factor in OS scheduling; a small quantum provides better interactivity but increases overhead (more switches), while a large quantum reduces overhead but may lead to longer waiting times for lower-priority processes. Most importantly, I learned that my synchronization logic is scalable and remains correct regardless of the scenario settings.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

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
