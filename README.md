# Deadlock

 When Threads Wait Forever

 PROVIDE ANALOGY ............ ATIQ BHAI

 Deadlock is a state in which atleast two threads/processes are stuck in a loop because they are dependent on each other they are stick beacuse resource that is needed by other processes/threads is held 
 by the first one .

 Because each thread holds a resource the other needs, and neither releases what they have until they get the other resource, they remain stuck forever unless something breaks the cycle.

 But can it occur randomly , well the Answer is NO !

 For deadlock to occur it needs few conditions.

 The Conditions are :
  * Mutual Exclusion — Only one thread can hold a resource at a time.
  * Hold and Wait — A thread holds at least one resource while waiting for others.
  * No Preemption — Resources cannot be forcibly taken from a thread; they must be released voluntarily.
  * Circular Wait — A circular chain of threads exists, where each waits for a resource held by the next.

   A Very important point is that for DEADLOCK to occur all four conditions should happen at same time , if any one of them breaks DEADLOCK is prevented .

   As an example :

   Suppose an attacker intentionally induces a deadlock in an EDR component to stall the tool.

   But this attack does not let any attacker get entry directly to kernel . 

# SPECIAL USER APC INJECTION CODE (DEADLOCK CODE)




    


   # IRQL 

   IRQL stands for INTERRUPT REQUEST LEVEL(IRQL) it is a windows kernel priority mechanism that determines which code can be executed at a given time . It also govenrs how interrupts(events) are handled and 
   
   how kernel components synchronize access to shared resources .

   Now we can get confused between IRQL and THREAD PRIORITY . But both are different thread priority is the in-charge of scheduling when the CPU is free , 

   while IRQL determines how events can get immediate attention regardless they are scheduled or not .

   Now we understood that IRQL is a key player in windows for system stability but what if we violate the rules of IRQL 

   well THE SYSTEM WILL CRASH .

   Since IRQL is a kernel-mode primitive sometimes it can be used for evasion or tampering by interrupting the execution handling in ways that can disrupt the monitoring by MALWARES .

   For Example :

   By delaying or racing callbacks , blocking certain APC , starving a monitoring thread . All of this degrades what EDR "sees" or how reliably it can respond .


# IRQL HIERARCHY 

Now IRQL has a hierarchy mechanism to determine which process will be executed first .There are 5 leavels to determine it.

The Five Levels are :
* PASSIVE LEVEL
* APC LEVEL
* DISPATCH LEVEL
* DIRQL (DEVICE IRQL)
* HIGH LEVEL

   Now let's understand what these 5 levels mean :

**1. PASSIVE LEVEL**

It is just a Normal Zone , in this level user mode application like chrome, notepad are executed and some standard Kernel Threads. It is the lowest priority level where most standard application code runs.

This level has full access to the pageable memory i.e the memory that can be swapped to dsik.

The threads are allowed to wait or sleep on synchronisation objects like Mutexes and Semaphores.

**CODE EXPLANATION** --ATIQ BHAI

**2. APC LEVEL**

It is a Software Interrupt Zone , in this level the activity that is executed is basic I/O Completeion Routines and Thread suspension. But why it is called APC level , it is because APC is a software interrut targeted at a specific thread.

For example :

When a file read finishes , the system uses special kernel APC to notify the thread that its data is ready .

There are some constraints with this level and that is You can still access pageabel memory , but this creates a "critical section" where the current APC cannot be interrupted by other APCs.

**CODE EXPLANATION** --ATIQ BHAI

**3. DISPATCH LEVEL**

It is a No Witing Zone , this level nurtures the Winodws Scheduler and Deferred Procedure Calls.
It handles the time-critical operations. Here there is no pageable memory as there is no waiting in this level , and a page falut here can cause a system crash . 

But what is Windows Scheduler - when windows raises the IRQL to

**CODE EXPLANATION** --ATIQ BHAI

**4. DIRQL (DEVICE IRQL)**

This level caters the hardwares of a System so it is also called The Hardware Zone or Device IRQL . In this level there is INterrupt Services Routines(ISRs) for devices like netowork cards (if people still use it ) , mice , or storage(100k GB).

The basic GOAL of this level is to Execute extremely quickly to capture data , queue a DPCand return control. But there are some contraints and these are that the Execution must be almost immediate , since no waiting or apging is allowed .

This takes us to our last Level

The DO NOT DISTURB ZONE 












 








# MAPPING THREE TYPES OF APC TO IRQL 

# WHAT COULD BE THE DEFENSIVE APPROACH TO THE PROCESS


# MOST ADVANCE FORM OF APC INJECTION ( A NOVEL APPROACH )

# EARLY CASCADE INJECTION 

# REQUIRED PREREQUISITES

 # FUNCTION ARCHITECTURE

 # TECHNICAL FLOW 

 # FAQs

   










 


