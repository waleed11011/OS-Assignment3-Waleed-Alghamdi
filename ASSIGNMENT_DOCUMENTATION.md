# Assignment 3 - Complete Documentation

**Student Name**: [Waleed Alghamdi]  
**Student ID**: [445050004]  
**Date Submitted**: [May 2, 2026, 10:30 PM]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: https://drive.google.com/file/d/1zXnmuRpaOVJODQnK_AB3odlupqvt3wFG/view?usp=drivesdk

**Video filename**: 445050004_Assignment3_Synchronization.mp4

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 2, 2026, 2:30 AM]
**What I implemented**: 
I started the assignment and set my student ID in the code.
**Challenges encountered**: 
No major challenges at the beginning, just understanding the structure of the code.
**How I solved it**: 
I read the code and understood where shared resources are used.
**Testing approach**: 
I ran the program to make sure everything works before adding synchronization.
**Time spent**: 
20 minutes
---

### Entry 2 - [May 2, 2026, 2:50 AM]
**What I implemented**: 
I implemented Task 1 by adding ReentrantLock to protect shared counters.
**Challenges encountered**: 
I was confused about where exactly to use the lock and how to apply it correctly.
**How I solved it**: 
I used try-finally blocks and wrapped the critical sections for all counters.
**Testing approach**: 
I ran the program multiple times and checked that the counter values are correct and consistent.
**Time spent**: 
40 minutes
---

### Entry 3 - [May 2, 2026, 3:10 AM]
**What I implemented**: 
I worked on Task 2 and protected the execution log using a separate lock.
**Challenges encountered**: 
Understanding why ArrayList is not thread-safe and how it can cause errors.
**How I solved it**: 
I added a new ReentrantLock (logLock) and used it inside logExecution method.
**Testing approach**: 
I ran the program and made sure no ConcurrentModificationException occurs.
**Time spent**: 
20 minutes
---

### Entry 4 - [May 2, 2026, 3:20 AM]
**What I implemented**: 
I implemented Task 3 using Semaphore to control CPU access.
**Challenges encountered**: 
I was not sure where to put acquire() and release().
**How I solved it**: 
I placed acquire() at the beginning of run methods and release() inside finally block.
**Testing approach**: 
I checked that only one process runs at a time.
**Time spent**: 
25 minutes
---

### Entry 5 - [May 2, 2026, 3:30 AM]
**What I implemented**: 
I tested the full program after completing all tasks.
**Challenges encountered**: 
Making sure all synchronization parts work together without errors.
**How I solved it**: 
I carefully checked locks and semaphore usage.
**Testing approach**: 
I ran the program several times and verified consistent output.
**Time spent**: 
15 minutes
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

Race condition happens when more than one thread uses the same data at the same time.

First example is contextSwitchCount. This variable is shared, and if more than one thread updates it together, some values can be lost and the result will be wrong.

Second example is executionLog. It is a shared ArrayList, and if many threads add to it at the same time, it may cause errors like ConcurrentModificationException or missing data.

This is a problem because the program will not give correct results.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

ReentrantLock is used to protect shared data, so only one thread can use it at a time.

Semaphore is used to control how many threads can use a resource.

In my code, I used ReentrantLock for counters and execution log.

I used Semaphore to allow only one process to use CPU at a time.
---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

Deadlock happens when threads are waiting for each other and nothing continues.

One way to prevent it is using try-finally to make sure the lock is released.

Another way is not keeping locks for a long time.

In my code, I used finally to release locks and semaphore.
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

I used one lock for all counters.

I chose this because it is simple and easy to use.

Using more locks can make the program faster, but it is harder.

The difference is that one lock is easier but slower, and multiple locks are faster but more complex.

Fine-grained locking gives better concurrency because different threads can work at the same time.

But for this assignment, one lock was enough.
---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
These variables are shared between threads. If multiple threads update them at the same time, the values may become incorrect.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
counterLock.lock();
try {
    contextSwitchCount++;
} finally {
    counterLock.unlock();
}

**Justification**: 
I used a lock to make sure only one thread updates the counters at a time and prevent race condition.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog
**Why it needs protection**: 
It is a shared ArrayList, and it is not thread-safe. Multiple threads writing to it may cause errors.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}

**Justification**: 
Using a lock prevents multiple threads from modifying the list at the same time.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control access to CPU and allow only one process at a time
**Number of permits and why**: 
1 permit, because only one process should run at a time
**Where implemented**: 
In run() and runToCompletion() methods
**Code snippet**:
cpuSemaphore.acquire();
try {
    // process execution
} finally {
    cpuSemaphore.release();
}

**Effect on program behavior**: 
Only one process can run at a time, which prevents conflicts and keeps execution correct.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync

**Results**: 
Every time I ran the program, the output was consistent. The number of completed processes and waiting time were correct.
**Why synchronization is necessary**: 
Without synchronization, shared variables like contextSwitchCount and executionLog can be updated at the same time by different threads. This can lead to wrong values or missing data.
**Conclusion**: 
Synchronization worked correctly and prevented race conditions.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
I ran the program while multiple threads were adding data to executionLog.
**Results**: 
No errors occurred during execution.
**What this proves**: 
This proves that executionLog is protected correctly using a lock.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
All processes should complete, and values should be logical
**Actual values**: 
All processes completed and values were correct
**Analysis**: 
The output matched expectations, which means the synchronization is working correctly.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 
To check if the program works under different conditions
**Results**: 
The program worked correctly in all cases
**What I learned**: 
Synchronization keeps the program stable even with different inputs
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned that synchronization is important when multiple threads share the same data.
Without synchronization, race conditions can happen and cause wrong results.
I understood how ReentrantLock protects shared variables.
I also learned how Semaphore controls access to resources.
At the beginning it was confusing, but after practicing it became easier.
This assignment helped me understand how threads work together safely.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Bank system where multiple users update the same account balance  
**Example 2**: 
Operating system scheduling where processes share CPU  
---

### How I would explain synchronization to others:

Synchronization means taking turns when using shared resources.
It is like only one person can use something at a time.
This helps avoid problems and keeps everything correct.

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/waleed11011/OS-Assignment3-Waleed-Alghamdi.git

**Number of commits**: 4

**Commit messages**: 
1. Task 3: Added semaphore to allow only one process to use CPU at a time
2. Task 2: Added lock to protect execution log
3. Task 1: Added lock to protect counters and avoid race conditions
4. Set my student ID : [445050004]

---

## Summary

**Total time spent on assignment**: 
4-5 hours
**Key takeaways**: 
1. Learned how to prevent race conditions
2. Understood how locks work
3. Learned how semaphore controls threads 

**Most challenging aspect**: 
Understanding where to use locks and semaphore 
**What I'm most proud of**: 
Completing all tasks and making the program work correctly  
---

**End of Documentation**
