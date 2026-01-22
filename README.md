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
<img width="2752" height="1494" alt="unnamed (8)" src="https://github.com/user-attachments/assets/0e947b32-9e10-4203-baff-5a740c388d8e" />

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



**2. APC LEVEL**

It is a Software Interrupt Zone , in this level the activity that is executed is basic I/O Completeion Routines and Thread suspension. But why it is called APC level , it is because APC is a software interrut targeted at a specific thread and the I/O routines are handled by the Threads of a System.

For example :

When a file read finishes , the system uses special kernel APC to notify the thread that its data is ready .

There are some constraints with this level and that is You can still access pageabel memory , but this creates a "critical section" where the current APC cannot be interrupted by other APCs.



**3. DISPATCH LEVEL**

It is a No Witing Zone , this level nurtures the Winodws Scheduler and Deferred Procedure Calls.
It handles the time-critical operations. Here there is no pageable memory as there is no waiting in this level , and a page falut here can cause a system crash . 

But what is Windows Scheduler - when windows raises the IRQL to



**4. DIRQL (DEVICE IRQL)**

This level caters the hardwares of a System so it is also called The Hardware Zone or Device IRQL . In this level there is INterrupt Services Routines(ISRs) for devices like netowork cards (if people still use it ) , mice , or storage(100k GB).

The basic GOAL of this level is to Execute extremely quickly to capture data , queue a DPCand return control. But there are some contraints and these are that the Execution must be almost immediate , since no waiting or apging is allowed .

This takes us to our last Level

**5. HIGH LEVEL**

The DO NOT DISTURB ZONE 

This level also caters the system , it halts all the system activities to execute a command . It has two very important Linked Process :

1.Bug Check
It is mainly used during crashes of system to safely write diagnoatic data(dump files) without interference from drivers or a scheduler(like in Level 3). 

2.NMI (Non-Maskable Interrupts) - It is Reserved for critical hardware failures like memory corruption.

# Mapping APC Types to IRQL Levels

After understanding the IRQL hierarchy, APC behavior can be mapped based purely on where execution is permitted.

**PASSIVE_LEVEL -- User APC Execution**

All user-mode APC payloads execute at PASSIVE_LEVEL.
This is the only IRQL at which user code can safely run.

Mapped APC types:

Simple User APC

Early Bird APC

Special User APC

Early Cascade Injection

Differences between these APC types affect delivery timing, not execution level.

**APC_LEVEL -- Kernel APC Execution**

Kernel APC routines execute at APC_LEVEL and are part of kernel scheduling and I/O mechanisms.

Mapped APC types:

 Normal Kernel APC
 
 Special Kernel APC

These APCs operate under stricter execution rules and do not involve user-mode payload execution.

Key Mapping Rule : 

 User APCs → PASSIVE_LEVEL
 
 Kernel APCs → APC_LEVEL

IRQL determines where execution is allowed, while APC type determines when the execution opportunity occurs.

Refer to the image below for better understanding 
<img width="2725" height="1387" alt="unnamed (10)" src="https://github.com/user-attachments/assets/320c83dc-4ae6-44a5-9c44-76407f6af131" />







 








# MAPPING THREE TYPES OF APC TO IRQL 

# WHAT COULD BE THE DEFENSIVE APPROACH TO THE PROCESS


# MOST ADVANCE FORM OF APC INJECTION ( A NOVEL APPROACH )

In APC injection we used to exploit different processes to execute our malicious code into the system that can get flagged by system's EDR , but have you ever thought as an attacker what if use insert the 
malicious code and queue it within the process without getting flagged by the EDR , Well Well Well Using Early Cascade Injection we can achieve this goal , but before starting with this Novel Approach we need
to know some terms that will help us understand this.

# REQUIRED PREREQUISITES

So Early Cascade Injection relies on very early _user-mode initialization_ . Now to understand what does this mean the following prerequisites are essential to know .

Since all the process Injection techniques are executed in a process we need to undertand 
_1._ Windows Process Creation (User-Mode Focus)

This will help us to understand what happens before a process actually "starts running".
* Before the process starts running it is created in suspended state
* Then Critical sections are initialized , it includes:
  * _.mrdata Section_
  * _.data Section_
* Then the core DLLs are loaded , for ex - kernel32.dll.
  
_One very important point to note is that all these initialization finishes before EDR user-mode hooks are fully active._


 We have already discussed APC injection in our previous blog **link to it** 
 Early Cascade Injection depends on advanced APC behavior.

To understand these behaviours we need to know some terms that we have already discussed in the blog or we will be discussing .

_2._ APC Fundamentals

- User APCs
  * It always execute at Passive_level (Level 1 of IRQL)
- Intra-process APCs
  * Since early cascade is a within process injection technique (later discussed in detail)
- ntdll!NtTestAlert
  * This function executes all the queued APCs when the thread enters an alertable state.
 
 _APCs must be queues before the process resumes , not after._


Now we should know 

_3._ WHY PASSIVE_LEVEL MATTERS 

it matters because ,
 * User APC execution executes in PASSIVE_LEVEL only.
 * Early Cascade executes only in user mode.
 * No kernel IRQL abuse is required (because of stealth advantage).


_4._ SHIM ENGINE

Shims engine or Windows Compatibility Engine is bascially a subsystem of windows that helps older or incompatible application to run on newer versions of Windows without modifying the application itself.

Now you must be thinking BUT WHY DOES THIS EXIST ?

Well it exists because when the windows evolves (windows 10 -> windows 11) , the APIs chnages , Security restriction increases this can lead to older application getting crashed or refuse to run , so instead of forcing developers to rewrite (which they will hate after every evolution)
Windows uses SHIMS.

This takes us to our last Prerequisite and that is ,

_5._ Payload (or Shellcode) Staging Concept

ECI (Early Cascade Injection) uses two payloads :

 1. PAYLOAD STUB
    * It acts a a initialozer for our main payload.
    * It is executed via the Shim Engine.
    * It Queues APCs internally.

   Now the MALICIOUS CODE
 
 2. Main Payload
    * It is the actual shellcode.
    * It is executed later after the process resumes via APC.


 
   
# EARLY CASCADE INJECTION 

Well now that we know the prerequisites we can start undertanding , how we can use this novel approach to inject the shellcode within the process .

Early Cscade Injection is a Process Injection Technique in Windows that was introduced by OUTFLANK in 2024 . It injects the shellcode during the process creation stage ( not after the process is created as in APC ) . 

This technique executes specifically in the *user-mode initialization phase* but before most EDR( Endpoint Detection and Response ) solution fully initialize their user-mode detection mechanisms.

This technique also avoids drawbacks such as cross - process APC queuing becuase in this execution occurs within a process so it does not need other process to work and loader-lock restrictions.





 # FUNCTION ARCHITECTURE

 # TECHNICAL FLOW 

 # FAQs

   










 


