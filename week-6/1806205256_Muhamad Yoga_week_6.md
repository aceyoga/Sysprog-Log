# System Programming Logbook Week  4

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [I/O Buffering](#I/O-Buffering)
- [The make](#The-make)
- [References](#References)

_________________________________________________________________________________________________________________________________________________________________________

## I/O Buffering

_____

A buffer is a temporary storage area used for I/O activities such as read/write. The explanations on how buffering works is written below:

In User space, there is a special stdio buffer used by stdio functions. This buffer is used for normal I/O operations, usually involving reading from terminal/file and writing to terminal/file. Some library function using the stdio buffer are printf() and fflush(). 

![buffer diagram](https://github.com/aceyoga/Sysprog-Log/blob/master/week-6/buffering1.png)

For example, when writing to a file from terminal/file input, the data is first placed into the stdio buffer, then flushed to a special kernel buffer cache(furthermore referred as kernel buffer). For programs that requires force flush, the fflush() is used. When the buffer is flushed, the data inside the buffer is placed into kernel buffer with write() system call or similar. Finally, the program writes the data inside kernel buffer to the disk by using open() system call to open the file. Usually when write() is called the process will automatically writes the data to the kernel buffer and to the file destination, thus the program does not need to call any other functions.

In practice, difference of buffer size directly impacts the performance of I/O performance. For example, we want to copy 2 files of same size to a different file, by using different buffer size. The computer running the tests uses Debian @VirtualBox for the OS and a Harddisk as it's main storage. 

```
time `dd if=/dev/urandom of=big1 bs=2048 count=25000`
time `dd if=/dev/zero of=big2 bs=1024 count=50000` 
```

In this test i'll use the dd function, which copies a file to another file with a configurable buffer size and target block count. The result is as follows:

![buffer test 1](https://github.com/aceyoga/Sysprog-Log/blob/master/week-6/buffering2.png)

![buffer test 2](https://github.com/aceyoga/Sysprog-Log/blob/master/week-6/buffering3.png)

The second test uses buffer size of 1/2 of the first test, and block count twice the first test. However, the second test runs at almost 3x the speed of the first test, and a lower CPU time. This shows that in harddisk, smaller buffer size leads to faster and more effective I/O runtime. It is caused by the faster read/write speed since smaller buffer size means the buffer will be filled faster and needs to be flushed more often.



_____

## The make

_____

The make command is a versatile utility used to maintain software projects / groups or programs. The make utility can automatically determine which piece of a large program needs recompiling, or which program needs to be recompiled because of changes. In order to use make, a makefile is needed. Makefile is a file that stores records and descriptions of relationships between programs inside the group, and stores the command/arguments for compiling/updating the files/programs.

This utility is not limited to programs, thus any files requiring update from other files can use make to simplify the update process. Once a group of files/program has a makefile written for them, a single make call is needed to update the whole group. 

_____

## References

_____

1. man time

2. man dd

3. man make

