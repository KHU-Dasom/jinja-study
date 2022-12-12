# Scheduling

## Basic Concepts
- **Multi Programming**: To maximize CPU utilization

### CPU-I/O Burst Cycle
![Alternating sequence of CPU and I/O bursts](https://user-images.githubusercontent.com/56192918/205545020-700e2be3-94fd-4f8e-8c2c-e031386f9cdd.png)
- Consists of a cycle of CPU execution and I/O wait
- Begins with a CPU burst and followed by an I/O burst

### CPU Scheduler
- CPU becomes idle, OS select one of the processes in the ready queue to be executed
- The selection process is carried out by the **CPU scheduler**
  - selects a process from the processes in memory that are ready to execute
  - allocates the CPU to that process

### Preemptive and Non-preemptive Scheduling
- CPU-scheduling decisions
  1. When a process switches from the running state to the waiting state
  2. When a process switches from the running state to the ready state
  3. When a process switches from the waiting state to the ready state
  4. When a process terminates


- Non-preemptive scheme(i, iv)
  - Once the CPU has been allocated to a process, the process keeps the CPU until it releases it either by terminating or by switching to the waiting state


- Preemptive scheme(ii, iii)
  - When data are shared among several processes, this can result in race conditions

### Dispatcher

![The role of dispatcher](https://user-images.githubusercontent.com/56192918/205567984-0772dac3-faf7-49cd-98af-40f7f6344390.png)
- Module that gives control of the CPU's core to the process selected by the CPU scheduler

- Involves
  1. Switching context from one process to another
  2. Switching to use mode
  3. Jumping to the proper location in the user program to resume that program

## Scheduling Criteria
1. CPU utilization ↑
2. Throughput ↑
3. Turnaround time ↓
4. Waiting Time ↓
5. Response Time ↓

## Scheduling algorithms
**CPU Scheduling** deals with the problem of ***deciding which of the processes in the ready queue is to be allocated the CPU's core***

### FCFS Scheduling
***First-Come, First-Served Scheduling***

![FCFS](https://user-images.githubusercontent.com/56192918/205582642-f52d636c-cc46-4618-9788-2eae5fb543c0.png)
Can be managed with a FIFO queue
- Advantage: Simple to write and understand
- Disadvantage: Average waiting time is often quite long(***Convoy Effect***)

![FCFS2](https://user-images.githubusercontent.com/56192918/205583592-6106c74d-a8c6-49a0-884a-fd48bdd1dd9e.png)
![FCFS3](https://user-images.githubusercontent.com/56192918/205583687-7382b416-77da-4a79-9ea8-5f8ebf55b6a9.png)
![FCFS4](https://user-images.githubusercontent.com/56192918/205583763-f9e9347d-133b-4619-914a-0595ab9ef250.png)

### SJF Scheduling
***Shortest Job First***
- When the CPU is available, it is assigned to the process that has the smallest next CPU burst
- If the next CPU bursts of two processes are the same, FCFS scheduling is used

![SJF1](https://user-images.githubusercontent.com/56192918/205586342-3301e64a-9602-4a43-86c3-0b904f1d7714.png)
![SJF2](https://user-images.githubusercontent.com/56192918/205586375-cb717440-40a5-4981-9c77-1d4755fc21ba.png)
- **Optimal**: Gives the minimum average waiting time for a given set of processes
  - No way to know the length of the next CPU burst
- Disadvantage: Starvation, Waste of resource, Hard to predict

### Round-Robin Scheduling
***Similar to FCFS scheduling, but preemption is added***
- **Time quantum** is defined
- Ready Queue(FIFO) is treated as **circular queue**
- The CPU scheduler goes around the ready queue, allocating the CPU to each process for a time interval of up to 1 time quantum

![RR1](https://user-images.githubusercontent.com/56192918/205591403-5326af8f-ceb1-4726-8caa-68e268470ada.png)
![RR2](https://user-images.githubusercontent.com/56192918/205591424-be8f89fb-ff3e-4d87-a25a-6e4554416a5f.png)
- (Consider time quantum of 4 milliseconds)
- Performance depends on the size of the time quantum

### Priority Scheduling
***SJF algorithm is a special case of priority scheduling algorithm***
- CPU is allocated to the process with the highest priority
- If the next CPU bursts of two processes are the same, FCFS scheduling is used

![Priority1](https://user-images.githubusercontent.com/56192918/206084768-ad364607-2cba-476f-8026-6e89adff3e69.png)
![Priority2](https://user-images.githubusercontent.com/56192918/206084782-3d49ecff-7d43-42a7-8d9f-90045afc87d3.png)

- Internal priorities
  - time limits
  - memory requirements
  - the number of open files
  - the ratio of average I/O burst to average CPU burst

- External priorities
  - importance of process
  - the type and amount of funds being paid for computer use
  - the department sponsoring the work

- Can be either preemptive or non-preemptive

- **Indefinite blocking/Starvation**
  - A steady stream of higher-priority processes can prevent a low-priority process from ever getting the CPU

- Solutions
  - **Aging**: Involves gradually increasing the priority of processes that wait in the system for a long time
  - **Round-Robin + Priority scheduling**
    - Round-Robin for equal priority processes
    
    ![Priority3](https://user-images.githubusercontent.com/56192918/206088571-c98323dc-bea0-4e9f-8cbd-e87ee805bca0.png)
    ![Priority4](https://user-images.githubusercontent.com/56192918/206088594-f5815dac-517d-4a66-879a-1e02d57dc353.png)
    
### Multilevel Queue Scheduling
***Separate queues for each distinct priority***

![Multilevel Queue](https://user-images.githubusercontent.com/56192918/207020040-7271d006-659e-4f02-9e06-f06b7e8891f4.png)
- Separate Queues for each priority
- Consists of **Foreground** and **Background** processes
  - Foreground: have priority over background processes(game)
  - Background: lower priority(messenger)
- Each queue might have its own scheduling algorithm

Fixed-priority preemptive scheduling
- scheduling among the queues

![Multilevel Queue2](https://user-images.githubusercontent.com/56192918/207020907-3cf9da15-9cba-4170-8206-739d8a16ef13.png)
- Absolute priority for each queue
- can be preemptive by higher priority queue

### Multilevel Feedback Queue Scheduling
***A process moves between queues***
Separate processes according to the characteristics of their CPU bursts 

![Multilevel Feedback Queue](https://user-images.githubusercontent.com/56192918/207024000-7a4c290a-e2a6-493e-9063-7b4f12eeaaaf.png)
- If a process uses too much CPU time, it will be moved to a lower-priority queue
- A process that waits too long in a lower-priority queue may be moved to a higher-priority queue

## Thread Scheduling
Threads: user-level, kernel-level
  - Kernel-level threads: scheduled by OS(not processes)
  - User-level threads: managed by library(kernel is unaware of them)

### Contention Scope
User-level vs Kernel-level: how they are scheduled
- User-level threads: many-to-one, many-to-many models -> **PCS(Process Contention Scope)**
  - Done according to priority
- Kernel-level threads: one-to-one, **SCS(System Contention Scope)**
공부 더

## Multi-Processor Scheduling
Multiple threads may run in parallel, load sharing becomes possible
- System architectures
1. Multiple CPUs
2. Multithreaded cores
3. NUMA systems
4. Heterogeneous multiprocessing

### Approaches to Multiple-Processor Scheduling
**AMP(Asymmetric Multi-Processing)**
- Master server(single processor): scheduling decisions, I/O processing
- Other processors: user code


- Advantage: simple(only one core accesses the system data structures->reducing the need for data sharing)
- Disadvantage: Performance may be reduced(master server can be a bottleneck)

**SMP(Symmetric Multi-Processing)**
- Each processor is self-scheduling
  - Scheduler for each processor examine the ready queue and select a thread to run
    1. All threads may be in a common ready queue
    2. Each processor may have its own private queue of threads

![SMP1](https://user-images.githubusercontent.com/56192918/206116207-a4b472f8-d80b-48da-b69c-fadb322a5ea2.png)
- a) Possible race condition on the shared ready queue
- b) workloads of varying sizes

### Multi Processors
Multiple computing cores on the same physical chip


**Memory stall**

![Memory stall](https://user-images.githubusercontent.com/56192918/206118917-6e78ee3e-0597-45e2-80d5-48146be8c3b3.png)
- Modern processors operate at much faster speeds than memory
- Cache miss
- To remedy, multithreaded processing cores in which **two or more hardware threads** are assigned to each core

![Hardware threads](https://user-images.githubusercontent.com/56192918/206120543-5e033b0b-e68d-4ceb-8d99-820d79e88cff.png)

CMT(Chip Multi-threading/Hyper Threading)

![CMT](https://user-images.githubusercontent.com/56192918/206121391-f4931455-e4e8-4d87-b5f5-b34fb1dab433.png)
- From the perspective of OS, there are eight logical CPUs

Two level of Scheduling

![Two levels of scheduling](https://user-images.githubusercontent.com/56192918/206123477-04356e8c-3745-4ec4-854b-a72125ed60e9.png)
1. Operating system chooses which software thread to run on each hardware thread
2. Each core decides which hardware thread to run

### Load Balancing
Keep the workload evenly distributed across all processors in an SMP system


Two approaches
1. Push migration 
   - Distributes the load by **pushing** threads from overloaded to idle


2. Pull migration
   - Idle processor **pulls** a waiting task from a busy processor

### Processor Affinity
***A process has an affinity for the processor on which it is currently running***

If the thread migrates to another processor due to load balancing
- Need Invalidating and repopulating caches -> High cost
  - Try to avoid migrating a thread from one processor to another
  - Keep a thread running on the same processor and take advantage of a **warm cache**

Two affinity strategies
1. Soft affinity
   - Operating system attempts to keep a process on a single processor, but not guaranteeing
2. Hard affinity
   - Allowing a process to specify a subset of processors on which it can run

- Linux implement soft affinity, but it also provides `sched_setaffinity()` system call which supports hard affinity

Processor affinity issues affected by main-memory architecture

![image](https://user-images.githubusercontent.com/56192918/207010544-d01f6ade-b208-41f3-bd14-00483509d60d.png)
- If NUMA-aware, thread can be allocated memory closest to where the CPU resides


### HMP
***Heterogeneous Multiprocessing***
Systems designed using cores that vary in terms of their clock speed and power management

- big.LITTLE
  - Higher-performance **big** cores are combined with energy efficient **LITTLE** cores
  - **Little** cores: low performance, need to run for longer periods(background tasks)
  - **big** cores: high performance, shorter durations(interactive applications)