Project 4: Bounded-buffer Producer Consumer Problem
CSE 330: Operating Systems - Spring 2024
Summary
In this project, we will use a kernel module to solve the classic synchronization problem, the producer-consumer problem. We will utilize the kernel threads and semaphores we built in Project 3 to implement our solution. This project will provide us with interesting kernel development experience and help us understand how to solve synchronization problems in real-world operating systems.
Description
In this project, you will implement a kernel module to calculate the total elapsed time of all the processes that belong to a given user in the system (In test scripts, we will automatically create a new user: test cse330). The producer and the consumer are kernel threads. The producer searches the system for all the processes that belong to a given user and adds the process information to the shared buffer. The consumer removes the processes from the buffer, collects the elapsed time of these processes, and outputs the total elapsed time in the kernel log. There is only one producer, but there can be multiple consumers. The producer and the consumers share a fixed-size buffer that contains process descriptors (task_struct)
In your module, you will use the module_param macro to pass input arguments to your kernel module. It takes four arguments & you MUST name your input parameters for the kernel module as follows & in the same order such that the test script can load the kernel module successfully. Failure to do so, your kernel module will not be loaded by the automated test script.
• buffSize: int, the buffer size
• prod: int, number of producers (0 or 1), and
• cons: int, number of consumers (a non-negative number).
• uuid: int, the UID of the user. In test scripts, we will automatically create a new user: test cse330

1. Producer Thread
The producer thread searches the system for all the processes that belong to a given user (In test scripts, we will automatically create a new user: test_cse330) and adds their task_struct to the shared buffer. There is only one producer in the system, and it exits after it has iterated through the entire task list.
The kernel stores all the processes in a circular doubly linked list called task list. Each element in the task list is a process descriptor of the type struct task_struct which contains all the information about a process. The below figure shows the process descriptors and task list.
We use the for_each_process macro to iterate through the task list to access each task_struct. The macro goes over all the task_struct in the task list one by one. The task_struct contains the process' PID in pid and the process' user's UID in cred->uid.val
for_ each process(struct task struct *p) II On each iteration, P
points to the next task in the list task struct *task;
task-›pid // PID of the process
task-›cred-›uid.val // UID of the user of the process
struct task struct.
struct task struct
struct task struct
struct task_struct
unsigned long state;
int prio;
unsigned long policy:
struct task_struct "parent; struct list head tasks;
pid_t pid;
the task list



The following code example calculates the number of processes in the task list.
#include‹linux/sched.h> #include <linux/sched/signal.h>
struct task_struct* p;
size_t process_counter = 0;
for_each_process(P) {
#process_counter;
Please refer to the sample code and see how it works:
https://github.com/CSE330-FALL-2023/Project-1-Template/tree/main/sample_code/process_struct B›
For each task, you will need pid, start_time, and boot_time to calculate elapsed time.
Note: If your module cannot exit, there is a thread still running or waiting on a semaphore. If you meet this, you need to power off your Virtual Machine and start it again. (Restart would not help).
2. Consumer Thread
The consumer thread reads out the task struct of the processes from the buffer and calculates the elapsed time for each process and the total elapsed time. For each task, you will need pid, start_time, and boot_time to calculate elapsed time. Multiple consumers in the system can work in an infinite loop; remember to check end_flag so they know when to stop. (Normally, we should check kthread_should_stop(), but it would cause deadlock.)
Requirements:
• Your module should be loaded and unloaded successfully.
• All the processes for the given user should be found by the producer and processed by the consumers, and no process should be produced or consumed more than once.
• The total elapsed time of all the processes should match the output of ps. Note that they will not exactly match due to the fact that they are not run at exactly the same time; a small difference is allowed.
# CSE330 Project 4 Bounded buffer Producer Consumer Problem

# 程序代做代写 CS编程辅导

# WeChat: cstutorcs

# Email: tutorcs@163.com

# CS Tutor

# Code Help

# Programming Help

# Computer Science Tutor

# QQ: 749389476
