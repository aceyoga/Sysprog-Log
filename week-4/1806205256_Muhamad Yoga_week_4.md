# System Programming Logbook Week  4

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [Process, Program, and Thread.](Process,-Program,-and-Thread.)
- [Forking](#Forking)
- [The ps command(Process Snapshot)](#The-ps-command(Process-Snapshot))
- [Linux Environment Variables](#Linux-Environment-Variables)
- [References](#References)

_________________________________________________________________________________________________________________________________________________________________________

## Process, Program, and Thread.

_____

A Program is a set of instructions written to solve a task. A process is a program in execution. Threads technically are also a process(or a part of a process) that are using the process resources for accomplishing a task. The main difference of a process and a thread is that processes are completely isolated in memory, and do not share memory between each other, while threads share the same memory with other threads. One process can have many threads(multithreading).

Example of a program: HelloWorld.java, Math.java, Something.java (Exists in secondary storage)
Example of a process/thread: HelloWorld(PID=10101), Math(PID=10009), systemd(PID=100), exists only in primary storage(RAM, Cache, Processor cache).

The highest PID that a process could have is 32768 by default, but it can be increased up to 4194304 on 64-bit systems. This also means that there can only be up to 4194304 process in a system at any moment. The information about this number is stored in /proc/sys/kernel/pid_max.

There are 3 types of abnormal process. They are:

1. Zombie Process

   These are processes that has already finished executing, but still havent fully terminated thus its PID will still show when ps(or other PID logging command is run) is invoked.

2. Orphan Process

   These are processes that still runs after their parent process terminates/finished. A process can be orphaned intentionally or unintentionally. Processes that are unintentionally orphaned is usually processes whose parent got terminated(force terminate due to errors/bugs/user command via kill), or crashed. Processes that are intentionally orphaned is usually used for “eternal service processes” or a very long-running task. One example of this is the Daemon Process(explained below).

3. Daemon Process

   These are special orphan process, is usually a background service process and runs out of user control. Usually daemons are run by booting processes, then got orphaned, and gets adopted by init process.

Differences between the three:

| Aspects       | Zombie P                                                     | Orphan P                                                     | Daemon P                                                     |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Use Resources | No                                                           | Yes                                                          | Yes                                                          |
| Lifespan      | Until reaped by parent                                       | Until it finishes running                                    | Indefinite(will terminate only when forced/system  shutdown occurs). |
| Side effects  | In large numbers, blocks new processes from running since there’s no PID  left for them. | Might crash the system if left parentless/becoming  zombie process. | Helps the system’s routine tasks(like systemd, crond,  syslogd, etc). |

To prevent the creation of zombie processes, we can do any of the following:  

1. Invoking wait(NULL) System Call on parent process.
2. Invoking signal(SIGCHLD,SIG_IGN) on parent process.
3. Using a Signal Handler, basically the first option but combines it with signal function.

When a process has finished running, it will either terminate using exit() or _exit(). The exit() is the standard process termination function, and terminates the calling function normally. Any open file descriptors will be flushed and closed, and the value of status & 0377 is returned to the parent. Meanwhile, _exit() function directly terminates the calling function, right after it is called. Just like exit(), the _exit() will also flush and close any open file descriptors related to the process. Both functions does not return anything. 

The exit() function is more Thread-safe than _exit(), due to the nature of Threading(child threads exiting with _exit() might cause errors to the parent thread, usually since a parent process expects a return value from the child instead of no return value at all).

______

## Forking

_____

Fork() is a function that creates a new process by duplicating the calling process. The newly created process is called the "child" process, and the original process is called its "parent". The parent and child process run in separate memory spaces, but until a write system call is invoked by the child, the child will still use the same resources as its parents. This is known as Copy-On-Write(COW). This method of memory usage increases efficiency and memory usage effectivity since there’s no new space is allocated to the child processes unless it invokes a write system call.

To identify which one is the parent, and which is the child, usually the fork() will return a 0 for the child, and the child PID is returned to the parent. Another way to identify it is by looking at the PPID(parent process ID) of the child, then matching it with another PID by using getpid() and getppid().

______

## The ps command(Process Snapshot) 

______

The ps command is used to capture a snapshot of currently running processes information. There are many options, but the usually used are:

1. ps(without any arguments). This calls the ps with default options, showing only 4 columns of information(PID, TTY(terminal), TIME, and CMD). This command will only show processes started by the user calling ps. Very useful for checking PIDs of processes that is manually started by the user.      

2. ps –e. This option will tell ps to list all processes that are currently running in the machine, from all users. The information shown is the same as ps, but the TTY column might be filled with a “?”, meaning that the process is not started via terminal. Useful if we want to monitor the whole machine, and/or finding processes that aren’t started by terminal.

3. ps –eF. The –F option(extra Full Format) will make ps show many more informations than both standard ps calls and ps –f calls. This option shows every single processes currently running, as well as other informations, for example:

   ![ps -eF](https://github.com/aceyoga/Sysprog-Log/blob/master/week-4/pseF.png)

   The description of each column is:

   UID: The user ID of the owner of this process.

   PID: The process ID of the process.

   PPID: The parent process ID of the process.

   C: The number of children the process has.

   STIME: Start time.

   TTY: The name of the console starting the process.

   TIME: The amount of CPU processing time that the process has used.

   CMD: The name of the command that launched the process.

There are 8 types of process states, and another 6 for the BSD format.

UNIX format:

D : uninterruptible sleep.
R : running/runnable(on queue).
S : interruptible sleep.
T : stopped by job control signal.
t : stopped by debugger during tracing.
W : paging (deprecated/not used anymore since kernel version 2.6+).
X : dead/terminated.
Z : defunct/zombie process.

BSD format

< : high-priority process.
N : low-priority process.
L : process with this state code has it’s pages locked into memory.
s : the process is a session leader.
l : the process is a multithreaded process(usually made by using pthreads).
\+ : the process is in a foreground process group.

_____

## Linux Environment Variables

_____

Environment variables are variables that are available system-wide and are inherited by all processes and shells. The environment variables are implemented as strings that represent key-value pairs. If multiple values are passed, they are usually separated by colon (:) characters.

There are 2 ways to permanently set an environment variable in Linux. One is using .bash_profile/.bashrc/similar file, and the other is by using the /etc/environment file. The main difference is that using .bash_profile/similar will only permanently set the contained variables only for the users with the file. Meanwhile, setting environment variable on /etc/environment forces everyone using the system with the variables inside the /etc/environment. For example:

1. Open .bash_profile using any text editor(could be vi, vim, nvim, or nano).

2. Write “export env_var_name=value”(the environment variable) directly inside the file.

3. Save and exit.

4. Use source command(or just “.” for some Linux systems) to reload the file so that the current and future terminal sessions can use the environment variables.

_____

## References

_____

1. man proc 5
2. https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux[
     ](https://www.bing.com/search?q=linux+environment+variables&cvid=caf49e7a54fe469794f6774785a92452&FORM=ANAB01&PC=U531#)
3. man fork 2
4. https://en.wikipedia.org/wiki/Copy-on-write
5. man getpid 2
6. man getppid 2
7. man ps 2
8. man top
9. https://www.tutorialspoint.com/zombie-vs-orphan-vs-daemon-processes
10. https://www.geeksforgeeks.org/zombie-processes-prevention/
11. https://devconnected.com/how-to-set-and-unset-environment-variables-on-linux
12. man _exit
13. man exit
