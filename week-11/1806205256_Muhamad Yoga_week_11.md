# System Programming Logbook Week  11

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [Daemon 101](#Daemon_101)
- [Daemon VS Background Processes](#Daemon_VS_Background_Processes)
- [Daemon Examples in Linux OS](#Daemon_Examples_in_Linux_OS)
- [The nohup, disown, and usage.](#The_nohup,_disown,_and_usage.)
- [How To Daemonize a Process](#How_To_Daemonize_a_Process)
- [Using Daemons for Scheduling
- [References](#References)

![nama gambar](https://github.com/aceyoga/Sysprog-Log/blob/master/week-10/namafile.tipe)

_________________________________________________________________________________________________________________________________________________________________________

## Daemon 101

_____

Daemon is a special process that has a long lifetime, usually from system startup until system shutdown. Daemons are usually created for a specific task which requires constant uptime. While the daemon is up and running, it needs to make sure that there's only one instance of it running at any given time. The daemon must also make sure that there's no memory leak happening. If it happens, it must clean it before something bad happens. When the system is shutting down, all daemon will receive a SIGTERM from init process 5 seconds before the SIGKILL sent by init process is received. In this 5 seconds, all daemon must immediately perform cleanup routines.

Daemons are required to close all standard file descriptors to prevent any unwanted problems, like running out of file descriptors, the daemon accidentally writing to the file descriptors, etc. Daemons usually have no controlling terminal. 

_____

## Daemon VS Background Processes

_____

Background Processes is a type of processes that runs in the background, mostly without getting user input. Usually background processes are used for logging, scheduling, and other automated task. In a Unix/Linux-based systems, a program an be initiated as background process can be directly created by using the "&" operator, and then the process will run in the background. The process can be reconnected to the parent terminal by using the *fg* utility, and resumes it's operation by using *bg* utility. The *jobs* utility can list all processes(including the background ones) that is associated with the current terminal, and is usually used to get the PID of background processes.

Meanwhile, Daemon Processes is a special type of background process with the longest uptime(usually from system startup-shutdown), minimal resource usage and usually no user input at all. The jobs utility can't be used to get the PID of daemon processes since the daemons themselves aren't associated with any terminal.	

_____

## Daemon Examples in Linux OS

_____

There are several examples of daemon processes in Linux OS. Some of them are:

- systemd
  - This daemon is the system and service manager for systems implementing Linux OS. Usually, this process takes the PID 1(as the init process), running and maintaining user-space services.
- syslogd
  - The syslogd runs a syslog program in the background as a daemon process, logging messages of system output as it happens.
- sshd
  - This daemon runs the ssh background process, providing secure and encrypted communications between two untrusted hosts over an insecure network. The daemon will listen for any incoming connection, then handles it with TCP/IP rules.

_____

## The nohup, disown, and usage.

_____

The nohup(no hang up) is a command used to run commands/start processes that stays running even after the terminal/shell invoking it terminates. Any command/processes that are run by using nohup will ignore any incoming hangup signals.

The disown is a command used to remove a job/background process from a job table(can be seen with jobs command). Disown have many parameters: -a will disown every job in the job table; %n will disown any processes with job id n; -r will disown any jobs currently running.

Both have their own uses. For example:

- The nohup command is usually used for starting processes that stays running and ignoring hangup signals.
- The disown command is used for already running processes to make them stays running in the background. Since the job is removed from the job list there will be no termination signal unless it got directly terminated/other errors.

There is another way of creating background processes, which is using "&".

_____

## How To Daemonize a Process

_____

Process Group is a....

In order to daemonize a process we must first...

Then, the setsid command needs to be called. This is done for....

The session created by setsid...

There is a way to differentiate a daemon process and a normal process. One of the easiest way is to look at its PPID(Parent PID). A daemon process will usually have a very small PPID, usually 1 which is the init process. Another way is to....

Reinitializing a daemon process can be done by using SIGHUP signal.

_____

## Using Daemons for Scheduling

_____

The cron is a system daemon used to run commands at a scheduled time. It will run any command specified in /etc/crontab file, with the following format:

minutes hours day_of_month month day_of_week command.

The 5 first argument can be supplied with an '*',  for selecting all values in that column. For example, you can run a backup of all your user accounts at 1 a.m every week with:

0 1 * * 1 tar -zcf /var/backups/home.tgz /home/

_____

## References

_____

1. Systems Programming Learning Material Slide 12-Daemons & Background Process
2. man daemon
3. [Daemon (computing) - Wikipedia](https://en.wikipedia.org/wiki/Daemon_(computing))
4. https://linuxize.com/post/how-to-run-linux-commands-in-background/
5. man systemd
6. [Syslog - Wikipedia](https://en.wikipedia.org/wiki/Syslog)
7. man sshd
8. [Chapter 13. Daemon Processes - Shichao's Notes](https://notes.shichao.io/apue/ch13/)
9. [Background process - Wikipedia](https://en.wikipedia.org/wiki/Background_process)
10. man jobs
11. man nohup
12. https://phoenixnap.com/kb/disown-command-linux
13. man setsid
14. [Chapter 9. Process Relationships - Shichao's Notes](https://notes.shichao.io/apue/ch9/#ensuring-the-successful-call-of-setsid)
15. man cron
16. man crontab
17. https://www.geeksforgeeks.org/cron-command-in-linux-with-examples/
18. https://www.geeksforgeeks.org/how-to-setup-cron-jobs-in-ubuntu/
19. 

