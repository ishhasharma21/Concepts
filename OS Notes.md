# Operating Systems

## 1. Concepts of Process & Threads
*Process is a program in execution. Thread is a segment of a process.* Programs with are dispatched from ready state & scheduled to execute are processes. 
https://www.geeksforgeeks.org/difference-between-process-and-thread/


## 2. CPU Scheduler & Scheduling algorithms
CPU Scheduling is a process that allows one process to use the CPU while another process is delayed (in standby) due to unavailability of any resources such as I / O etc, thus making full use of the CPU. The purpose of CPU Scheduling is to make the system more efficient, faster, and fairer.

Process Scheduling is the process of the process manager handling the removal of an active process from the CPU and selecting another process based on a specific strategy. Process Scheduling allows the OS to allocate CPU time for each process. Another important reason to use a process scheduling system is that it keeps the CPU busy at all times. This allows you to get less response time for programs. 

**Context switching is swapping programs in and out of CPU**. CPU scheduling is the process of deciding which process will own the CPU to use while another process is suspended.

![image](https://user-images.githubusercontent.com/107466664/180600620-8375db2f-8d59-4a46-9899-b464475e6867.png)


![image](https://user-images.githubusercontent.com/107466664/180600705-66da7a8a-13f1-4f74-bfb6-e0b531773bfd.png)

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

![image](https://user-images.githubusercontent.com/107466664/180603427-208f044b-89c2-47c1-8d4e-c143746bef42.png)


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

![image](https://user-images.githubusercontent.com/107466664/180604023-44f3a79c-c1d0-40dd-9012-139fb73324e2.png)

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

![image](https://user-images.githubusercontent.com/107466664/180604350-26c4a22a-d44f-47b1-a4eb-f556634c333d.png)


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
## 10. Monitors & Deadlocks 
## 11. Message Passing 

## 12. Deadlock (detection, prevention, recovery, avoidance)
## 13. Inter Process Communication

https://www.geeksforgeeks.org/inter-process-communication-ipc/

Sometimes dependency of processes (execution of one process affects the other) will increase the efficiency and moduarity of programs. These processes communicate 
using : 
- Message Passing 
- Shared Memory 

## 16. Memory management  
## 17. Memory partitioning 
## 18. Memory Swapping 
## 19. Contiguous memory allocation
## 20. Paging with segmentation
## 21. Segmentation with paging 
## 22. Process creation 
## 23. Page replacement algorithms 
## 24. Allocation of frames
## 25. Disk Scheduling 
