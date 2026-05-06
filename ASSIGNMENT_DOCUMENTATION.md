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

### Entry 5 - [2026-05-05, 10:00 PM]
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

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

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

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

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
