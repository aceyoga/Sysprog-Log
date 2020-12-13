# System Programming Logbook Week  13

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [Kernel Module & Driver](#Kernel_Module_&_Driver)
- [Devices 101](#Devices_101)
- [Device Driver Flow](#Device_Driver_Flow)
- [Kernel Module 101](#Kernel_Module_101)
- [References](#References)

_________________________________________________________________________________________________________________________________________________________________________

## Kernel Module & Driver

_____

A Module is a program/code that extends the functionality of a kernel, and works like an extension(can be loaded and unloaded at runtime). In linux, a module is usually referred as LKM or KMOD. LKM is usually used to extend/add support for new hardwares/filesystems, or for adding new system call functions. When the LKM is not needed anymore, it can be unloaded to free memory and space.

A Driver is a program/code that acts as the hardware's "Driver/Controller", bridging between kernel and hardware. A large part of a running kernel consists of driver programs, while the rest is usually programs for scheduling, memory management, etc.

The Module/Driver acts as an interface bridging between the system itself and the corresponding hardware, mapping driver functions with specific hardware functions. All apps/programs at user level can use the standard function defined by the module/driver to use hardware.

However, some drivers(like hardware, memory, and player drivers) are essential and must be built statically into the kernel file on disk. Some other drivers can be built as a LKM for easy loading/unloading. 

The standard practice is to build drivers as kernel modules when possible, instead of building them statically. There are several reasons:

- LKM can be unloaded to reduce memory/space usage.
- LKM does not contribute to boot process slowdown.
- Some LKM are not drivers.

And there are several reasons to build modules statically:

- Some Drivers are essential(like Memory Drivers) to the kernel.
- In embedded systems, building all drivers statically is usually ideal.



_____

## Devices 101

_____

There are 3 types of devices:

- Block Devices
  - Devices that have physical addressable storage media, like disks.
  - Perform I/O operations using strategy routine.
- Character Devices
  - Devices that do not have physical addressable storage media.
  - Perform I/O operations in a bytestream.
- Network Devices
  - Special devices that handles networking related operations, like communication between systems/computers, interactions between hardware and a computer network.

Each devices have a Major and Minor number. A major number identifies the driver associated with the device. A minor number is used by a driver to differentiate between devices.

_____

## Device Driver Flow

_____

How device driver works when an application at user level tries to use hardware can be described by using the illustration taken from Sysprog Week 13 PreTest document below:

![driverflow](https://github.com/aceyoga/Sysprog-Log/blob/master/week-13/driverflow.jpg)

The user program(Say, Writer) runs 4 command. The open, read, write and close. All 4 command is a system call, which gets passed to the device driver routines by the kernel, described below:

1. When open is called, the kernel receives the open system call and calls the device driver's open function, and the driver interacts with the hardware, tells the kernel that the location is accessible, gives an fd integer, then the kernel passes the fd integer to the fd variable.
2. When read is called, the kernel receives and calls the driver's read function, interacting with the hardware, and returning the requested data from read parameters.
3. When write is called, the kernel receives and calls the driver's write function, interacting with the hardware, and writes the data specified from write parameters.
4. When close is called, the kernel calls the driver's close function and tells the hardware to close the fd.

_____

## Kernel Module 101

_____

A module consists of Entry Code and Function Library. Entry code is the two functions that is called when the module is loaded(The init function) and when the module is unloaded(The cleanup function). The Function library is all other function that the module have. 

Since stdlib cannot be used in modules, due to user/kernel space issues, we use printk for printing messages in kernel programs. There are 8 types of information that can be printed using printk:

KERN_EMERG, KERN_ALERT, KERN_CRIT, KERN_ERR, KERN_WARNING, KERN_NOTICE, KERN_INFO, KERN_DEBUG

Before trying to make/compile modules, first we need to install all requirements/dependencies:

build-essential, linux-headers-3.2.0-4-486

_____

## References

_____

1. Systems Programming Learning Material Slide: Kernel Module & Module Compilation 1 & 2
2. https://docs.oracle.com/cd/E19620-01/805-3024/6j2sumi0r/index.html
3. https://www.oreilly.com/library/view/linux-device-drivers/0596000081/ch03s02.html
4. http://www.makelinux.net/ldd3/chp-4-sect-2
5. man insmod
6. man lsmod
7. man rmmod
8. man modinfo

