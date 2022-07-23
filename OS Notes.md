# Operating Systems

## Concepts of Process & Threads
*Process is a program in execution. Thread is a segment of a process.* Programs with are dispatched from ready state & scheduled to execute are processes. 
https://www.geeksforgeeks.org/difference-between-process-and-thread/

## CPU Scheduler & Scheduling algorithms
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

## Principles of Concurrency 
## Race Condition 
When multiple processes access the same resources or share variables, the output of those variables might be incorrect. Since several processes race to manipulate the 
resource, the chronology becomes very important to determine what is correct and what is not. 

***Race condition occurs withi a critical section***

### Critical Section Problem 
- Can be accessed by only 1 process at a time. 
- Critical section problem means a way of synchronizing the process to share resources and memory spaces without creating data inconsistencies. 

## Mutual Exclusion 
## Semaphores
## Implementation of Semaphore 
## Classical Problems in Concurrent programming 
## Critical Regions 
## Monitors & Deadlocks 
## Message Passing 
## Readers and Writers problems
## Producer & Consumer problem 
## Deadlock (detection, prevention, recovery, avoidance)
## Dining Philosophers problem 
## Memory management  
## Memory partitioning 
## Memory Swapping 
## Contiguous memory allocation
## Paging with segmentation
## Segmentation with paging 
## Process creation 
## Page replacement algorithms 
## Allocation of frames
## Disk Scheduling 
