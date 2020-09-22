# System Programming Logbook Week  1

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [Definition of System Programming](#Definition-of-System-Programming)
- [System Programming vs Application Programming](#System-Programming-vs-Application-Programming)
- [Scripting Programming Language](#Scripting-Programming-Language)
- [Operating Systems and Kernel](#Operating-Systems-and-Kernel)
- [Some frequently-used Linux commands](#Some-frequently-used-Linux-commands)
- [Program vs Process](#Program-vs-Process)
- [Proc File System](#Proc-File-System)
- [Filesystem Hierarchy and address paths](#Filesystem-Hierarchy-and-address-paths)
- [Partition](#Partition)
- [References](#References)

_________________________________________________________________________________________________________________________________________________________________________

## Definition of System Programming

_____

By definition, system programming is the activity of programming computer system software, that is aimed for producing software & software platforms that provides services for other software. System programming requires a great degree of hardware knowledge and awareness. The goal of system programming is to create software that can use available resource efficiently and effectively, since even a slight improvement can directly impact performance of software that uses it. System programming products include, but not limited to Operating Systems, SaaS applications, Graphic Engines, Game Engines, Component Drivers, etc.

______

## System Programming vs Application Programming

_____

In the other hand, application programming is the activity of designing and building software that is aimed to accomplish a specific task, sometimes with high efficiency and effectiveness. Application programming requires a great degree of computational awareness, mathematical knowledge(for implementing algorithms), problem solving skills, etc. Some examples of Application programming products are Internet Browsers, Microsoft Office, Teleconference apps, Chatting apps, etc.

The difference can be written in the following table, based on indicator categories.

| Indicator             | Systems Programming                                          | Application Programming                                      |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Goal/Intent           | To create a software that can use available resource efficiently and effectively, and be used as a service/interface for other software to function. | To create a software capable of accomplishing a specific task, sometimes with high efficiency and effectiveness depending on requirements and target. |
| Awareness & Knowledge | Hardware-related topic(Hardware specifications, capabilities, limitations, etc) | Software-related topic(Algorithms, problem-solving, development methodology, etc) |
| Language level        | Binary code and low level languages                          | Generally uses high-level languages                          |
| Examples              | Operating Systems, SaaS applications, Graphic Engines, Game Engines, Component Drivers, etc. | Internet Browsers, Microsoft Office, Teleconference apps, Chatting apps, etc. |

______

## Scripting Programming Language

______

Scripting language is an interpreter-based programming language that doesn’t require compilation, as opposed to compiled language that requires a compiler to compile it's source code to a runnable executable file(like Java and C). Interpreted programs reads, analyzes and checks the program line by line(or statements by statements), and will stop running at the specific line/statement that creates an error. Some examples of scripting languages are Python, Javascript and PHP. This makes interpreted programs run slower than compiled programs in most cases.

_____

## Operating Systems and Kernel

_____

Operating System is a system software that controls the operation of a computer and directs the processing of programs (as by assigning storage space in memory and controlling input and output functions). 

A Kernel is the core of an operating system, having complete access and control to everything in the system. The kernel is tasked with executing processes, handling interrupt calls, managing resource and memory usage, acting as security layer to avoid unwanted system calls, etc. 

When an operating system is loaded into memory, the kernel loads first and remains in memory until the operating system is shut down again. The kernel is responsible for low-level tasks such as disk management, task management and memory management.

Nowadays, most CPU have 2 modes, User Mode and Kernel Mode. Their descriptions are:

#### User Mode

This is the normal mode where all applications run inside a specific virtual space, and there is no application that shares the same virtual space with other. This is done to prevent unwanted problem like data overriding(Critical Point), running into race condition, etc. If an application happens to crash, only that application and it's virtual space is affected.

#### Kernel Mode

If an application enters/have the Kernel Mode active, it gains unlimited access to all system resources. If an application crashed while in this mode, it could potentially cause the whole system to crash, depending on the severity of the crash. Usually only a certain applications are allowed to enter kernel mode, like the kernel itself and device drivers.

_____

## Some frequently-used Linux commands

_____

### man

According to the manual page, man is an interface to the on-line reference manual(manual pager). man accepts several arguments, like the program/utility/function name, sections, etc. man is used to check descriptions, how-to-use, and usage examples.

### cd

The cd command(change directory) is used for changing the terminal's running directory. It accepts either an absolute path, or a relative one. 

### ls

The ls command prints all contents inside the current terminal working directory. It accepts arguments like -a(all), -d(directory), -l(long listing format), etc.

### cat

The cat command concatenate files given in the argument, then print it on the standard output. It accepts multiple file as input, and options such as -n(number all output lines).

### echo

The echo command is similar to cat, but instead of printing concatenated file contents, echo also accepts primitive string line as input.

### grep

The **g**lobally search for a **r**egular **e**xpression and **p**rint matching lines(or grep in short) is a versatile utility similar to Windows search command. But instead of using GUI, grep uses terminal/CLI and accepts regex as the search parameter.

## Program vs Process

_____

A program is a set of instructions used to perform a specific task, or used as a program that runs another program(like operating systems running another program to accomplish something). A process is a program that is being executed by the computer. Programs are usually stored as files in a secondary/tertier storage(Harddisk, Flashdisk, SD Cards) since it's not being actively used(thus not requiring high performance and expensive storage) while processes are stored in RAM/Cache/Processor registers since it’s active and running(thus requiring high performance and expensive storage). A program can last indefinitely in storage(unless it got deleted/the storage is damaged), but processes only lasts until it finishes running/stopped. 

The processes memory segmentation differs from each language, but there are 4 standard segmentation, which is:

- Code, this segment contains all instructions used for execution.
- Data, this segment stores data initialized by the instructions in the code segment.
- Stack, this segment stores variables and other data, like addresses and current process state.
- Heap, this segment stores data allocated by the process itself.

_____

## Proc File System

_____

The proc file system(procfs) is a special filesystem in Unix-like OS that contains system-related information in a hierarchical file structure(Similar to the GNU/Linux FHS). This filesystem makes kernel dynamic data access activity more convenient and easy than directly accessing the kernel memory. By default, the procfs is stored in the /proc directory. 

Currently, procfs stores information both related and unrelated to processes, inside files like:

- cpuinfo(stores the cpu-related information such as processor id, name, model, clockspeed, etc). 
- meminfo(memory-related information).
- modules(list of currently loaded kernel modules).
- devices(list of character and block devices sorted by device ID).

_____________

## Filesystem Hierarchy and address paths

_____

The UNIX OS and all its descendant uses the Tree Hierarchy filesystem, where everything is placed inside the root directory("/"). In GNU/Linux distributions, the filesystem evolved to File Hierarchy Standard(FHS) which the newest release is the FHS 3.0 in 2015. It still uses the same directory lists as the original UNIX filesystem, but it adds new directory like /media and /mnt.

The table below lists all directories in the FHS.

| Name   | Description                                                  |
| ------ | ------------------------------------------------------------ |
| /      | Primary hierarchy root and root  directory of the entire file system hierarchy. |
| /bin   | Essential command binaries that need  to be available in single-user mode; for all users, e.g., cat, ls, cp. |
| /boot  | Boot loader files, e.g., kernels,  initrd.                   |
| /dev   | Device files, e.g., /dev/null,  /dev/disk0, /dev/sda1, /dev/tty, /dev/random. |
| /etc   | Host-specific system-wide  configuration files.              |
| /home  | Users' home directories, containing  saved files, personal settings, etc. |
| /lib   | Libraries essential for the binaries  in /bin and /sbin.     |
| /media | Mount points for  removable media.                           |
| /mnt   | Temporarily mounted filesystems.                             |
| /opt   | Optional  application software packages.                     |
| /proc  | Virtual filesystem providing process  and kernel information as files. In Linux, corresponds to a procfs mount.  Generally, automatically generated and populated by the system, on the fly. |
| /root  | Home directory for the root user.                            |
| /run   | Run-time variable data: Information  about the running system since last boot, e.g., currently logged-in users and  running daemons. |
| /sbin  | Essential system binaries, e.g., fsck,  init, route.         |
| /srv   | Site-specific data served by this  system, such as data and scripts for web servers, data offered by FTP  servers, and repositories for version control systems. |
| /sys   | Contains information about devices,  drivers, and some kernel features. |
| /tmp   | Temporary files (see also /var/tmp).  Often not preserved between system reboots and may be severely  size-restricted. |
| /usr   | Secondary hierarchy for read-only user  data; contains the majority of (multi-)user utilities and applications.  Should be shareable and read-only. |
| /var   | Variable files: files whose content is  expected to continually change during normal operation of the system, such as  logs, spool files, and temporary e-mail files. |

Notes: Table taken from https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard, to avoid misconceptions and misunderstandings.

In navigating through the filesystems, an address is used as links that tells where something is located. There are 2 types of address paths, Absolute and Relative paths. Absolute path is a complete address path of a file/folder that is relative to the root address("/" in Linux, or in windows, for example: "C:/"). Absolute path can be a very short, or very long string, depending on the depth of the file/folder and the length of name each folder in the path passes, thus making it less efficient and effective. Meanwhile, relative path is a partial address path of a file/folder that is relative to the current working directory of the terminal/CLI, instead of the full address like absolute path. This makes relative paths the choice to use in websites, and programs working in a single location.

_____

## Partition

_____

Partition is a logical division of a storage drive(Usually HDD/SSD) that enables an OS to have several drives based on a single drive. A common example of OS using partitions is Windows, which divides the HDD/SSD into 2 partitions, namely C:/(For OS, other system and program files) and D:/(User drive), sometimes even E:/ for larger storage drives. To create/modify/delete partitions, a user needs access to a Partition Editor program.

Linux uses multiple partitioning scheme called paging. This allows Linux(and its users) to assign a new filesystem to chosen directories. This also allows an unlimited amount of partitions to be created(as long as there's still storage, so not totally unlimited).

_____

## References

_____

https://en.wikipedia.org/wiki/Systems_programming

https://en.wikipedia.org/wiki/Computer_programming

https://www.geeksforgeeks.org/whats-the-difference-between-scripting-and-programming-languages/

http://www.linfo.org/operating_system.html

http://www.linfo.org/kernel.html

https://www.tutorialspoint.com/User-Mode-vs-Kernel-Mode

https://en.wikipedia.org/wiki/Procfs 

https://en.wikipedia.org/wiki/Unix_filesystem

https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard

http://www.differencebetween.net/technology/difference-between-absolute-and-relative-path/

https://www.techopedia.com/definition/5544/partition
