# System Programming Logbook Week  2

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [System Call](#System-Call)
- [System Call Execution Steps](#System-Call-Execution-Steps)
- [Library Functions](Library Functions)
- [The GNU C Library (glibc)](#The-GNU-C-Library-(glibc))
- [System Call and Library Functions Error Handling](#System-Call-and-Library-Functions-Error-Handling)
- [References](#References)

_________________________________________________________________________________________________________________________________________________________________________

## System Call

_____

A System Call is a special gateway that separates the kernel from normal processes, such that for a normal process to do something that requires kernel authority, the process can delegate the requests to a System Call function so that the kernel perform requested activity on the process's behalf. The System Call API lets programs and process able to access and perform kernel-level tasks without actually forcing itself to enter kernel mode. One such API service example are I/O System Calls(write, open, etc).

______

## System Call Execution Steps

_____

![System Call Execution Step](week-2/System Call Execution Step.png)

Firstly, the application calls an Library Function that wraps the system call. The function must pass all arguments to the system call trap-handling routine, via the stack. The function then will copy the arguments received from the stack to registers specific to the system call.    Then, the function inserts the system call number to a specific CPU register for identifying which system call is called. The function executes the trap machine instruction(that int 0x80), which causes the processor to switch to kernel mode and execute the code in the system’s trap vector at 0x80.

The kernel responds to the trap, and invokes a system call routine to handle the trap. The system call saves the previous register values filled by the function to the kernel stack, checks the system call number validity, and then invokes the requested system call. If the system call receives arguments, it will first validate the arguments. Then the system call performs the requested task, then returns the status number to the trap handler routine. The register values from the kernel stack is restored, and the system call return value is placed on the stack. Then it returns to the wrapper function and switches the processor back to user mode, then returns again to the application.

______

## Library Functions

______

The library functions are a set of functions that include and expands the standard C library. There are so many functions, each with its own specific purpose ranging from simply opening a file and printing the content to terminal(cat), to complex functions like math functions. While many library functions doesn't use System Calls(since the context is different thus system call is not needed), some functions are built on top of a system calls(like a wrapper function). Some implementations is done to provide a more "user-friendly" function and more diverse function capable of accepting many/complex options that suits the user's needs, like the printf() function that enables output formatting, instead of the write() system call it wraps that only provides byte output service.

Some Library functions that wraps one/more system calls are:

```
printf(), wraps write() system call;
fopen(), wraps open() system call;
malloc(), wraps the brk() and sbrk() system call.
```



_____

## The GNU C Library (glibc)

_____

The GNU C Library project provides *the* core libraries for the GNU system and GNU/Linux systems, as well as many other systems that use Linux as the kernel. These libraries provide critical APIs including ISO C11, POSIX.1-2008, BSD, OS-specific APIs and more. These APIs include such foundational facilities as `open`, `read`, `write`, `malloc`, `printf`, `getaddrinfo`, `dlopen`, `pthread_create`, `crypt`, `login`, `exit` and more.

_____

## System Call and Library Functions Error Handling

_____

In C, errors are not automatically handled. If an error occurs in a C program, the program will return an error number according to what error occurs. The program is expected to handle errors on their own, by relying on the error numbers provided by the process. The errno command(from moreutils package) lists all 133 error numbers, listed below:

```
yoga@LocalYoga:~$ errno -l
EPERM 1 Operation not permitted
ENOENT 2 No such file or directory
ESRCH 3 No such process
EINTR 4 Interrupted system call
EIO 5 Input/output error
ENXIO 6 No such device or address
E2BIG 7 Argument list too long
ENOEXEC 8 Exec format error
EBADF 9 Bad file descriptor
ECHILD 10 No child processes
EAGAIN 11 Resource temporarily unavailable
ENOMEM 12 Cannot allocate memory
EACCES 13 Permission denied
EFAULT 14 Bad address
ENOTBLK 15 Block device required
EBUSY 16 Device or resource busy
EEXIST 17 File exists
EXDEV 18 Invalid cross-device link
ENODEV 19 No such device
ENOTDIR 20 Not a directory
EISDIR 21 Is a directory
EINVAL 22 Invalid argument
ENFILE 23 Too many open files in system
EMFILE 24 Too many open files
ENOTTY 25 Inappropriate ioctl for device
ETXTBSY 26 Text file busy
EFBIG 27 File too large
ENOSPC 28 No space left on device
ESPIPE 29 Illegal seek
EROFS 30 Read-only file system
EMLINK 31 Too many links
EPIPE 32 Broken pipe
EDOM 33 Numerical argument out of domain
ERANGE 34 Numerical result out of range
EDEADLK 35 Resource deadlock avoided
ENAMETOOLONG 36 File name too long
ENOLCK 37 No locks available
ENOSYS 38 Function not implemented
ENOTEMPTY 39 Directory not empty
ELOOP 40 Too many levels of symbolic links
EWOULDBLOCK 11 Resource temporarily unavailable
ENOMSG 42 No message of desired type
EIDRM 43 Identifier removed
ECHRNG 44 Channel number out of range
EL2NSYNC 45 Level 2 not synchronized
EL3HLT 46 Level 3 halted
EL3RST 47 Level 3 reset
ELNRNG 48 Link number out of range
EUNATCH 49 Protocol driver not attached
ENOCSI 50 No CSI structure available
EL2HLT 51 Level 2 halted
EBADE 52 Invalid exchange
EBADR 53 Invalid request descriptor
EXFULL 54 Exchange full
ENOANO 55 No anode
EBADRQC 56 Invalid request code
EBADSLT 57 Invalid slot
EDEADLOCK 35 Resource deadlock avoided
EBFONT 59 Bad font file format
ENOSTR 60 Device not a stream
ENODATA 61 No data available
ETIME 62 Timer expired
ENOSR 63 Out of streams resources
ENONET 64 Machine is not on the network
ENOPKG 65 Package not installed
EREMOTE 66 Object is remote
ENOLINK 67 Link has been severed
EADV 68 Advertise error
ESRMNT 69 Srmount error
ECOMM 70 Communication error on send
EPROTO 71 Protocol error
EMULTIHOP 72 Multihop attempted
EDOTDOT 73 RFS specific error
EBADMSG 74 Bad message
EOVERFLOW 75 Value too large for defined data type
ENOTUNIQ 76 Name not unique on network
EBADFD 77 File descriptor in bad state
EREMCHG 78 Remote address changed
ELIBACC 79 Can not access a needed shared library
ELIBBAD 80 Accessing a corrupted shared library
ELIBSCN 81 .lib section in a.out corrupted
ELIBMAX 82 Attempting to link in too many shared libraries
ELIBEXEC 83 Cannot exec a shared library directly
EILSEQ 84 Invalid or incomplete multibyte or wide character
ERESTART 85 Interrupted system call should be restarted
ESTRPIPE 86 Streams pipe error
EUSERS 87 Too many users
ENOTSOCK 88 Socket operation on non-socket
EDESTADDRREQ 89 Destination address required
EMSGSIZE 90 Message too long
EPROTOTYPE 91 Protocol wrong type for socket
ENOPROTOOPT 92 Protocol not available
EPROTONOSUPPORT 93 Protocol not supported
ESOCKTNOSUPPORT 94 Socket type not supported
EOPNOTSUPP 95 Operation not supported
EPFNOSUPPORT 96 Protocol family not supported
EAFNOSUPPORT 97 Address family not supported by protocol
EADDRINUSE 98 Address already in use
EADDRNOTAVAIL 99 Cannot assign requested address
ENETDOWN 100 Network is down
ENETUNREACH 101 Network is unreachable
ENETRESET 102 Network dropped connection on reset
ECONNABORTED 103 Software caused connection abort
ECONNRESET 104 Connection reset by peer
ENOBUFS 105 No buffer space available
EISCONN 106 Transport endpoint is already connected
ENOTCONN 107 Transport endpoint is not connected
ESHUTDOWN 108 Cannot send after transport endpoint shutdown
ETOOMANYREFS 109 Too many references: cannot splice
ETIMEDOUT 110 Connection timed out
ECONNREFUSED 111 Connection refused
EHOSTDOWN 112 Host is down
EHOSTUNREACH 113 No route to host
EALREADY 114 Operation already in progress
EINPROGRESS 115 Operation now in progress
ESTALE 116 Stale file handle
EUCLEAN 117 Structure needs cleaning
ENOTNAM 118 Not a XENIX named type file
ENAVAIL 119 No XENIX semaphores available
EISNAM 120 Is a named type file
EREMOTEIO 121 Remote I/O error
EDQUOT 122 Disk quota exceeded
ENOMEDIUM 123 No medium found
EMEDIUMTYPE 124 Wrong medium type
ECANCELED 125 Operation canceled
ENOKEY 126 Required key not available
EKEYEXPIRED 127 Key has expired
EKEYREVOKED 128 Key has been revoked
EKEYREJECTED 129 Key was rejected by service
EOWNERDEAD 130 Owner died
ENOTRECOVERABLE 131 State not recoverable
ERFKILL 132 Operation not possible due to RF-kill
EHWPOISON 133 Memory page has hardware error
```

Some System Calls might return -1 value even if the call was successful. This false alarm can be handled by setting the global variable errno to 0 before calling, and checking the value if the System Call returns -1. If the errno value changes, then an error has occurred and the errno should now contain the error number.

_____

## References

_____

1. The Linux Programming Interface: A Linux and UNIX System Programming Handbook (Page 43:44, 46, 48:49)
2. https://man7.org/linux/man-pages/man2/syscalls.2.html
3. http://www.gnu.org/software/libc/
4. https://man7.org/linux/man-pages/man3/errno.3.html
5. https://man7.org/linux/man-pages/man2/setpriority.2.html

