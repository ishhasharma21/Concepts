# Operating Systems

## 1. Concepts of Process & Threads
*Process is a program in execution. Thread is a segment of a process.* Programs with are dispatched from ready state & scheduled to execute are processes. 


https://www.geeksforgeeks.org/difference-between-process-and-thread/


## 2. CPU Scheduler & Scheduling algorithms
CPU Scheduling is a process that allows one process to use the CPU while another process is delayed (in standby) due to unavailability of any resources such as I / O etc, thus making full use of the CPU. The purpose of CPU Scheduling is to make the system more efficient, faster, and fairer.

Process Scheduling is the process of the process manager handling the removal of an active process from the CPU and selecting another process based on a specific strategy. Process Scheduling allows the OS to allocate CPU time for each process. Another important reason to use a process scheduling system is that it keeps the CPU busy at all times. This allows you to get less response time for programs. 

**Context switching is swapping programs in and out of CPU**. CPU scheduling is the process of deciding which process will own the CPU to use while another process is suspended.

**Objectives of Process Scheduling Algorithm:**

- Utilization of CPU at maximum level. Keep CPU as busy as possible. 
- Allocation of CPU should be fair. 
- Throughput should be Maximum. i.e. Number of processes that complete their
execution per time unit should be maximized. Minimum turnaround time, i.e. time taken by a process to finish execution should be
the least. 
- There should be a minimum waiting time and the process should not starve in the
ready queue. 
- Minimum response time. It means that the time when a process produces the first
response should be as less as possible.


- Arrival Time: Time at which the process arrives in the ready queue. -
-  Completion Time: Time at which process completes its execution. 
-  Burst Time: Time required by a process for CPU execution. 
-  Turn Around Time: Time Difference between completion time and arrival time.
 > Turn Around Time = Completion Time - Arrival Time


- Waiting Time (W.T): Time Difference between turn around time and burst time.
 > Waiting Time = Turn Around Time - Burst Time

***Read Scheduling algorithms *** 



### There are three types of process schedulers:

- Long term or Job Scheduler
> New processes to Ready State. Affects degree of multiprogramming (how many processes in ready state)

*Effect on performance*

The long term scheduler is responsible for creating a balance between the I/O bound(a process is said to be I/O bound if the majority of the time is spent on the I/O operation) and CPU bound(a process is said to be CPU bound if the majority of the time is spent on the CPU). So, if we create processes which are all I/O bound then the CPU might not be used and it will remain idle for most of the time. This is because the majority of the time will be spent on the I/O operation.
So, if we create processes that are having high a CPU bound or a perfect balance between the I/O and CPU bound, then the overall performance of the system will be increased.


- Short term or CPU Scheduler 
> Ready -> Running. All scheduling algorithms are used here. 

If short term scheduler messes up and selects a bad process, all other processes will go into starvation. 


- Medium-term Scheduler
> Running -> Ready / Blocked State. 
Swap from main memory to secondary memory. 



## 3. Principles of Concurrency WATCH VIDEOO!!!!!!!!!!!!!!!!!!!!!!!!

> Execution of multiple instruction sets at the same time. 

- When several process threads are running in parallel. 
- Communication between them happens through shared memory space or message passing. 
- It helps in techniques like coordinating execution of processes, memory allocation and execution scheduling for maximizing throughput. 



## 4. Race Condition 
When multiple processes access the same resources or share variables, the output of those variables might be incorrect. Since several processes race to manipulate the 
resource, the chronology becomes very important to determine what is correct and what is not. 

***Race condition occurs withi a critical section. Sequnce is the race condition***

### Critical Section Problem 
- Can be accessed by only 1 process at a time. 
- Critical section problem means a way of synchronizing the process to share resources and memory spaces without creating data inconsistencies. 

```
do {
	entry section 
	
		//critical section
		
	exit section
		
		remainder section
} 
while (true)
```


In the entry section, the process requests for entry in the Critical Section.

Any solution to the critical section problem must satisfy three requirements:

- **Mutual Exclusion** : If a process is executing in its critical section, then no other process is allowed to execute in the critical section.
- **Progress**: If no process is executing in the critical section and other processes are waiting outside the critical section, then only those processes that are not executing in their remainder section can participate in deciding which will enter in the critical section next, and the selection can not be postponed indefinitely.
- **Bounded Waiting**: A bound must exist on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.

## 5. Mutual Exclusion (Mutex)

The primary task of OS is to get rid of race conditions. This is done using Mutual Exlusion. No 2 processes should enter their critical section at the same time.
Critical Section is when they access a shared memory space. 


## 6. Semaphores
Semaphores are integer variables within threads which are used to solve the critical section problem using *wait* and *signal* operations. 
"Wait" operation decrements integer value wheareas "signal" increments it. 

```
wait(int S) {
    while(S >= 0)
        S --;
}

signal(int S){
    S ++; 
}
```

- *Counting Semaphores* : unrestricted domain | used to count resources 
- *Binary Semaphores* : 0 and 1 | usually signals if you can access critical section or not

Binary Semaphores = Mutex Locks


**Advantages**
- Easily ensures no 2 processes being in the critical section. 
- machine independent, since code is also machine independent.

**Disadvantages**
- Lower priority process can access over a higher priority process
- Complicated, must be implemented with care and alertness
- Their use leads to loss  in modularity

## 7. Implementation of Semaphore 

Reader - Writer & Producer - Consumer problems can be solved using semaphores.

https://user-images.githubusercontent.com/107466664/180604350-26c4a22a-d44f-47b1-a4eb-f556634c333d.png




## 8. Classical Problems in Concurrent programming 
The classical problems of synchronization are as follows:

### Producer Consumer problem
We have a buffer of fixed size. A producer can produce an item and can place in the buffer. A consumer can pick items and can consume them. We need to ensure that when a producer is placing an item in the buffer, then at the same time consumer should not consume any item. In this problem, **buffer is the critical section.**

To solve this problem, we need two counting semaphores – Full and Empty. “Full” keeps track of number of items in the buffer at any given time and “Empty” keeps track of number of unoccupied slots. 

PRODUCER 

```
do {
  //placing an item in buffer
  wait (mutex);
  wait (empty);            // reduce  the number of empty buffer
  
  //out of critical section
  signal (mutex);
  signal (full);           // increase the number of full buffer
 }
 while (true)
 
```

Mutex = 1 : Critical Section can be accessed
Mutex = 0 : Cannot be accessed 

CONSUMER 

```
do {
   //consuming an item from buffer
   wait (mutex);
   wait (full);
   
   // out of critical section 
   signal (mutex); 
   signal (full);
  }
while (true)
```



### Sleeping barber problem
There is a barber shop which has one barber, one barber chair, and n chairs for waiting for customers if there are any to sit on the chair.

If there is no customer, then the barber sleeps in his own chair.
When a customer arrives, he has to wake up the barber.
If there are many customers and the barber is cutting a customer’s hair, then the remaining customers either wait if there are empty chairs in the waiting room or they leave if no chairs are empty.

```
Semaphore Customers = 0;
Semaphore Barber = 0;
Mutex Seats = 1;
int FreeSeats = N;

Barber {
	while(true) {
			/* waits for a customer (sleeps). */
			down(Customers);

			/* mutex to protect the number of available seats.*/
			down(Seats);

			/* a chair gets free.*/
			FreeSeats++;
			
			/* bring customer for haircut.*/
			up(Barber);
			
			/* release the mutex on the chair.*/
			up(Seats);
			/* barber is cutting hair.*/
	}
}

Customer {
	while(true) {
			/* protects seats so only 1 customer tries to sit
			in a chair if that's the case.*/
			down(Seats); //This line should not be here.
			if(FreeSeats > 0) {
				
				/* sitting down.*/
				FreeSeats--;
				
				/* notify the barber. */
				up(Customers);
				
				/* release the lock */
				up(Seats);
				
				/* wait in the waiting room if barber is busy. */
				down(Barber);
				// customer is having hair cut
			} else {
				/* release the lock */
				up(Seats);
				// customer leaves
			}
	}
}
```


### Dining Philosophers problem

The Dining Philosopher Problem states that K philosophers seated around a circular table with one chopstick between each pair of philosophers. There is one chopstick between each philosopher. A philosopher may eat if he can pick up the two chopsticks adjacent to him. One chopstick may be picked up by any one of its adjacent followers but not both. 

```
process P[i]
 while true do
   {  THINK;
      PICKUP(CHOPSTICK[i], CHOPSTICK[i+1 mod 5]);
      EAT;
      PUTDOWN(CHOPSTICK[i], CHOPSTICK[i+1 mod 5])
   }
```


### Readers and writers problem

The readers-writers problem relates to an object such as a file that is shared between multiple processes. Some of these processes are readers i.e. they only want to read the data from the object and some of the processes are writers i.e. they want to write into the object.

The readers-writers problem is used to manage synchronization so that there are no problems with the object data. For example - If two readers access the object at the same time there is no problem. However if two writers or a reader and writer access the object at the same time, there may be problems.

To solve this situation, a writer should get exclusive access to an object i.e. when a writer is accessing the object, no reader or writer may access it. However, multiple readers can access the object at the same time.

This can be implemented using semaphores. 


## 9. Critical Regions 

When one or more processes aaccess the same shared resource, it is the critical region. Group of code here needs to be executed ATOMICALLY. 

> Atmocity is unbreakability i.e. an uninterrupted option. 


## 10. Monitors & Deadlocks 

Deadlock is when a set of processes are blocked because every process is holding some resource and waitinng for others. 

**4 necessary conditions**
- Mutal Exclusion (resource cannot be shared by multiple processes)
- Hold and Wait (of resources)
- No Preemption (cannot be taken away forcefully)
- Circular Wait (can make a circular diagram)



## 11. Message Passing WATCH VIDEOOOOOOOOOOOOOOOOOOOOOOO !!!!!!!!!!!!!!!!!!!!

In this method, processes communicate with each other without using any kind of shared memory. If two processes p1 and p2 want to communicate with each other, they proceed as follows:
 

Establish a communication link (if a link already exists, no need to establish it again.)
1. Start exchanging messages using basic primitives.
2. We need at least two primitives: 
– send(message, destination) or send(message) 
– receive(message, host) or receive(message)

The header part is used for storing message type, destination id, source id, message length, and control information. The control information contains information like what to do if runs out of buffer space, sequence number, priority. Generally, message is sent using FIFO style.



## 12. Deadlock (detection, prevention, recovery, avoidance)
**DETECTION** using a resource allocation graph & Allocation Matrix

https://user-images.githubusercontent.com/107466664/180607471-3f56150c-ee33-408b-9b17-7d80b01059bf.png


https://user-images.githubusercontent.com/107466664/180607488-023d94d9-0bd5-45b0-935f-bcfd841e1ca5.png

**PREVENTION**
Can be prevented by eliminating any of the 4 given conditions : 
- Mutual Exclusion
- Hold & Wait
- Circular Wait
- No Preemption

https://www.geeksforgeeks.org/deadlock-prevention/

**RECOVERY**
- Preempt resources
- Rollback to safe state
- Kill a process 
- Kill all processes

**AVOIDANCE**
Banker's algorithm
> Bankers’s Algorithm is resource allocation and deadlock avoidance algorithm which test all the request made by processes for resources, it checks for the safe state, if after granting request system remains in the safe state it allows the request and if there is no safe state it doesn’t allow the request made by the process.

## 13. Inter Process Communication

https://www.geeksforgeeks.org/inter-process-communication-ipc/

Sometimes dependency of processes (execution of one process affects the other) will increase the efficiency and moduarity of programs. These processes communicate 
using : 
- Message Passing 
- Shared Memory 

## 16. Memory management  

Kind of method / functionality to manage various kinds of memory. How can we have an efficient utilisation of memory? 

- CPU is directly connected to cache, registers and RAM. 
- Secondary memory is way slower than all the above memory spaces. 
- However, increasing spaces for the main memory will increase the cost of systems exponentially. 
- CPU cannot execute processes in the secondary memory, they have to come in the RAM.
- Degree of multiprogramming : when you fetch programs from secondary to main memory (RAM), try to fetch multiple programs instead of one. 
- Multiprogramming directly proportional to utilisation of CPU
- Every process has 
	a) CPU time request
	b) I / O operations.
  In cases of I/O operations, processes get blocked and come back when they need CPU space. Till then, other processes are executed. 
 
 ```
 CPU Utilisation = 1 - K <sup>no. of processes </sup>
 RAM = 4 mb
 Process = 4 mb
 No of processes = RAM / Process Size
 		 = 4 / 4 
		 = 1
		 
 I/O operations (K) = 70 % 
 Thus Utilisation = 1 - K 
 		  = 30 % 
		  
if RAM = 16 mb, 
no. processes = 16 / 4
	      = 4
	      
Thus, Utilisation = 1 - K<sup>4</sup>
		  = 93 %
```
   
## 17. Memory partitioning 

### Fixed Size Partitioning

https://www.youtube.com/watch?v=bK-VhQA512c


- IMPORTANT : CONTIGUOUS MEMORY ALLOCATION ONLY
- no. of partitions are fixed. 
- size of the partitions may or may not be the same. 
- OS is always the first process loaded into the RAM
- Spanning is not allowed
- Spanning = some part of process in one partition and the rest in other.

**DISADVANTAGES**
1. Internal Fragmentation 
2. Cannot accomodate processes with sizes bigger than maximum partitionn size
3. Degree of multiprogramming is affected since fixed number of partitions and no spanning
4. External Fragmentation

***Internal Fragmentation***
When a smaller process (eg : 2 mb) is fit into a bigger partition (eg : 4 mb), the 2 mb space remaining is the internal fragmentation. The wastage due to not fitting 
of processes. 

***External Fragmentation***
Although the sum of all internal fragmentation >= new process size, we still cannot accomodate it due to contiguous allocation constraint. 


### Variable Size Partitioning

https://www.youtube.com/watch?v=JdPmsrYqRDY

Creating partitions at the runtime and not before. 

**All disadvantages except EXTERNAL FRAGMENTATION of fixed size partitioning are solved here and makes it their advantage**

**Disadvantage**
1. Holes. If a process is completed and swapped out, we have a hole of that much space i.e. external fragmentation
2. Can use compaction (free memory differentiated from used up spaces). This leads to the problem of having to suspend already running processes. Since the process
   will have to be copied and moved to another address space. 
 3. Compaction is also time consuming (imagine having to move millions of bytes to another address space)


### Memory Allocation Methods 
Allocation to holes 

https://www.youtube.com/watch?v=N3rG_1CEQkQ


***First Fit***
- First big enough location to fit the process.
 - Simple & Fast
- weird shaped holes  

***Next Fit***
- Keep a pointer onn the previously allocated hole. Start searching first fit wise from the previously allocated hole. 
- faster than first fit
- internal fragmentation

***Best Fit***
- Traverse entire list and pick the one with least internal fragmentation.
- very less internal fragmentation individually
- but these tiny holes will be useless in accommodating more processes.
- traverses entire list, so slow

***Worst Fit***
- Traverse entire list to find the one with most internal fragmentation.
- slower since entire list has to be traversed
- holes big enough to fit in other processes


## 18. Memory Swapping 

Swap from main memory -> secondary memory & from secondary memory -> main memory TEMPORARILY.
If a main memory process need to go for I/O wait, it will be swapped out to secondary memory and another process will be swapped in in its place. After the I/O
wait, it will get swapped in from the secondary memory to the main memory.

## 19. Contiguous memory allocation
The enitre process together or nothing at all. Non - contiguous has process divivded into segments which can be stored seperately. 

## 20. Paging with segmentation
## 21. Segmentation with paging 
## 22. Process creation 
## 23. Page replacement algorithms 
## 24. Allocation of frames
## 25. Disk Scheduling 
