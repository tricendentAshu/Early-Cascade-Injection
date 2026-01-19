# Early-Cascade-Injection




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

   To get direct entry to kernel there is IRQL 


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

   

   










 


