# System Programming Logbook Week  3

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [System Call](#System-Call)
- [File Descriptor, Inode Table, File Description](#File-Descriptor,-Inode-Table,-File-Description)
- [The Linux Programming Interface
- [I/O System Calls](#I/O-System-Calls)
- [References](#References)

_________________________________________________________________________________________________________________________________________________________________________

## System Call

_____

A System Call is a special gateway that separates the kernel from normal processes, such that for a normal process to do something that requires kernel authority, the process can delegate the requests to a System Call function so that the kernel perform requested activity on the process's behalf. The System Call API lets programs and process able to access and perform kernel-level tasks without actually forcing itself to enter kernel mode. One such API service example are I/O System Calls(write, open, etc).

______

## Linux Stat Command

_____

Stat is a Linux command used to display information about a file/system file. There are several variations of this commands(lstat, fstat) in which usage can be describe as follows:

- stat
  - Standard stat() command, accepts pathname as arguments. Will still display file pointed by pathname even if the pathname is a symbolic link.
- lstat
  - This variation will display information related to the pathname itself if the pathname is a symbolic link.
- fstat
  - This variation will only display  a certain file information, specified by the supplied file descriptor.

The complete stat() information field and description can be seen below:

![Stat Structure](https://github.com/aceyoga/Sysprog-Log/blob/master/week-3/Stat-Structure.jpg)

![Stat Description](https://github.com/aceyoga/Sysprog-Log/blob/master/week-3/Stat-Desc.JPG)

______

## File Descriptor, Inode Table, File Description

______

A File Descriptor is a special integer constant variable used for I/O tasks. A process trying to perform I/O will pass the file descriptor of the file to the kernel through system call(Usually write()), then the kernel will access the file and perform requested I/O tasks delegated by the process. 

Inode(Index Node) Table is a table in Unix filesystem that stores information about files and directories, like access mode, filetype, etc. 

File Description is a collection of information related to a File, like name, size, type, etc which can be retrieved by using stat() or its variations.

Below is the relationship diagram between File Descriptors, File Table, and Inode Table:

![Relationship diagram](https://upload.wikimedia.org/wikipedia/commons/f/f8/File_table_and_inode_table.svg) 

There's a special process that doesn't have file descriptor. These processes are called Daemons. Daemons are also known as Background Processes, since it is detached from all terminal. Daemons are usually created by a process forking a child process then exits immediately(or directly run by init()) or created by daemon() function, which is used by programs that wants to detach from controlling terminal to run as a background process as system daemons.

_____

## The Linux Programming Interface

_____

![TLPI COver](https://man7.org/tlpi/cover/TLPI-front-cover.png) 

The Linux Programming Interface is the most comprehensive documentation of Linux and UNIX System Programming in existence, with 1552 pages, 115 diagrams, 88 tables, nearly 200 example programs, and over 200 exercises in total.

_____

## I/O System Calls

_____

In Unix and Linux, there are 3 standard descriptors used for I/O. They are:

- 0 (STDIN)
- 1 (STDOUT)
- 2 (STDERR)

The 4 key of I/O System calls are the following command:

- fd = open (pathname, flags, mode) 
- numread = read (fd, buffer, count)
- numwritten = write (fd, buffer, count)
- status = close (fd)

There are 8 different Flags for file access modes, listed below:

![Flag Modes](https://github.com/aceyoga/Sysprog-Log/blob/master/week-3/Flag-Modes.JPG)

There are several consequences if we don’t close a file at all:

- System will ran out of file descriptors/handles, failing any more open() system calls.
- System will close the file automatically(If there’s a buffered data in queue for that file, it’ll be lost unless the user knows exactly where the data is on the memory.)

_____

## References

_____

1. man fstat 2
2. https://man7.org/linux/man-pages/man2/fstat.2.html
3. https://en.wikipedia.org/wiki/File_descriptor
4. https://en.wikipedia.org/wiki/Inode
5. https://en.wikipedia.org/wiki/Daemon_(computing)
6. man daemon 1
7. https://man7.org/tlpi/
8. [https://scele.cs.ui.ac.id/pluginfile.php/80771/mod_resource/content/1/03%20-%20The%20Stat%20System%20Call.pdf](https://scele.cs.ui.ac.id/pluginfile.php/80771/mod_resource/content/1/03 - The Stat System Call.pdf), page 17-18, 24
9. https://stackoverflow.com/questions/8175827/what-happens-if-i-dont-call-fclose-in-a-c-program
10. https://stackoverflow.com/questions/13982478/what-is-file-hole-and-how-can-it-be-used
11. https://www.systutorials.com/handling-sparse-files-on-linux

