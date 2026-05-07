# Assignment 3 - Complete Documentation

**Student Name**: [Leen Sultan]  
**Student ID**: [445052128]  
**Date Submitted**: [7/5/2026]

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
I added synchronization mechanisms using ReentrantLock and Semaphore inside the SharedResources class.

**Challenges encountered**: 
I was confused about where to place the locks and how to protect shared variables correctly.

**How I solved it**: 
I created separate locks for the counters and used lock() and unlock() inside try-finally blocks.

**Testing approach**: 
I ran the program several times and checked whether the counters changed correctly.

**Time spent**: 
45 minutes

---

### Entry 2 - [Date, Time]
**What I implemented**: 
I synchronized the execution log list to prevent concurrent modification issues.

**Challenges encountered**: 
The ArrayList was not thread-safe and could be modified by multiple threads at the same time.

**How I solved it**: 
I protected the executionLog.add() operation using a lock.

**Testing approach**: 
I executed multiple processes simultaneously and verified that log entries were added correctly.

**Time spent**: 
30 minutes

---

### Entry 3 - [Date, Time]
**What I implemented**: 
I added the CPU semaphore acquisition and release inside the run() method.

**Challenges encountered**: 
I forgot to release the semaphore at first, which could lead to deadlock.

**How I solved it**: 
I placed cpuSemaphore.release() inside the finally block.

**Testing approach**: 
I checked that all processes completed execution without freezing.

**Time spent**: 
35 minutes

---

### Entry 4 - [Date, Time]
**What I implemented**: 
I tested the scheduler behavior with different process burst times and priorities.

**Challenges encountered**: 
Some processes required multiple quantums before completion.

**How I solved it**: 
I verified the Round Robin logic and remaining time calculations.

**Testing approach**: 
I compared the output with expected scheduling behavior.

**Time spent**: 
40 minutes

---

### Entry 5 - [Date, Time]
**What I implemented**: 
I completed the final documentation and verified all GitHub commits.

**Challenges encountered**: 
Organizing the explanation and screenshots clearly.

**How I solved it**: 
I reviewed all commits and summarized each implementation step.

**Testing approach**: 
I reran the project and checked the final statistics.

**Time spent**: 
25 minutes

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[The first race condition existed in the contextSwitchCount variable. Multiple threads could increment the counter at the same time using contextSwitchCount++. Since incrementing is not an atomic operation, some updates could be lost, producing incorrect results.

The second race condition existed in the executionLog list. Multiple threads could call executionLog.add() simultaneously while using a normal ArrayList, which is not thread-safe. This could lead to corrupted log data or exceptions such as ConcurrentModificationException.

To solve these problems, I used ReentrantLock to protect shared resources and ensure that only one thread accesses the critical section at a time.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[ReentrantLock is used for mutual exclusion, meaning only one thread can access a critical section at a time. I used locks to protect shared counters and the execution log.

A Semaphore controls access to a limited number of resources. In my code, I used a semaphore called cpuSemaphore to limit process execution and simulate CPU access.

Locks protect data consistency, while semaphores manage resource availability.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock happens when two or more threads wait forever for resources held by each other.

The first prevention technique I used was placing unlock and semaphore release operations inside finally blocks. This guarantees that resources are released even if an exception occurs.

The second technique was keeping lock usage simple and short to avoid nested locks and circular waiting.

These methods ensured that the program completed successfully without freezing.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I used one shared lock for all counter variables, which is considered coarse-grained locking.

I chose this approach because it was simpler and easier to manage in this assignment. Using one lock reduced the complexity of synchronization and minimized the chance of mistakes.

The advantage of coarse-grained locking is simplicity and easier debugging. However, it reduces concurrency because only one thread can update any counter at a time.

Fine-grained locking would allow multiple threads to update independent counters simultaneously, improving performance and concurrency.

Since the three counters are independent variables, fine-grained locking would provide better concurrency because threads would not block each other unnecessarily.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
public static final ReentrantLock lock = new ReentrantLock();

public static void incrementContextSwitch() {
    lock.lock();
    try {
        contextSwitchCount++;
    } finally {
        lock.unlock();
    }
}
```

**Justification**: 
The lock ensures that only one thread updates the counters at a time.

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
public static void logExecution(String message) {
    lock.lock();
    try {
        executionLog.add(message);
    } finally {
        lock.unlock();
    }
}
```

**Justification**: 
The lock prevents simultaneous modifications to the shared list.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();

try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 
The semaphore ensured controlled execution and prevented multiple processes from accessing the CPU simultaneously.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**: 
The program consistently completed all 11 processes successfully, and synchronization statistics remained valid in every run.

**Why synchronization is necessary**: 
Without synchronization, shared counters and execution logs could produce incorrect results due to race conditions.

**Conclusion**: 
Synchronization successfully protected shared resources and ensured stable execution.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
I executed multiple threads while continuously adding log entries.

**Results**: 
No exceptions occurred after adding synchronization.

**What this proves**:
The execution log is safely protected against concurrent access.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
- All processes should complete
- Context switches should increase correctly
- Waiting time should be calculated properly

**Actual values**: 
- Total Context Switches: 22
- Total Completed Processes: 11
- Total Waiting Time: 438815ms
- Average Waiting Time: 39892ms

**Analysis**: The actual results matched the expected scheduling behavior.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**:
To verify scheduler correctness under different workloads. 

**Results**: 
Processes with larger burst times required multiple quantums before completion.

**What I learned**: Round Robin scheduling distributes CPU time fairly among processes.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[I learned how synchronization prevents race conditions in multithreaded programs. Shared resources such as counters and lists require protection to avoid inconsistent data. I understood the difference between locks and semaphores and how each one serves a different purpose. Locks are useful for protecting critical sections, while semaphores control access to limited resources. I also learned how deadlocks can happen and how try-finally blocks help prevent them. Testing concurrent programs is important because errors may not appear every time. This assignment improved my understanding of thread safety and CPU scheduling concepts.]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking systems where multiple users access the same account balance simultaneously.

**Example 2**: Operating systems where multiple processes compete for CPU resources.

---

### How I would explain synchronization to others:

[Synchronization is like using a key for a shared room. If many people try to enter the room at the same time, things may become disorganized. The key allows only one person to enter and use the room safely before another person enters. In programming, synchronization ensures that threads access shared data safely and correctly.]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 5

**Commit messages**: 
1. 
2. Added synchronization locks for shared counters
3. Added semaphore synchronization in run method
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
