# System Programming Logbook Week  12

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [Booting Terminologies](Booting_Terminologies)
- [Boot Sequence](Boot_Sequence)
- [Customizable Startup Script](Customizable_Startup_Script)
- [BIOS and MBR vs UEFI and GPT](BIOS_and_MBR_vs_UEFI_and_GPT)
- [The Init and Systemd](The_Init_and_Systemd)
- [Kernel 101](Kernel_101)
- [Kernel Compiling](Kernel_Compiling)
- [References](#References)

![nama gambar](https://github.com/aceyoga/Sysprog-Log/blob/master/week-10/namafile.tipe)

_________________________________________________________________________________________________________________________________________________________________________

## Booting Terminologies

_____

In the booting process, there are many programs, procedures, and concepts being used. Some of the most important are:

- Loader
  - A Program that moves bits from disk (usually) to memory and then transfers CPU control to the newly “loaded” bits (executable).
- Bootloader
  - Program that loads the chosen kernel from Boot Manager.
- Boot PROM(BIOS/UEFI)
  - Persistent code that is “already loaded” on power-up. Act as the program that manages pre-booting process.
  - BIOS is shorthand for Basic Input/Output System. 
  - UEFI is shorthand for Unified Extensible Firmware Interface.
- Boot Manager
  - The program that lets you choose which init program to use.

_____

## Boot Sequence

_____

The boot sequence can be described effectively by the following picture taken from Sysprog slide 8:

![bootseq](https://github.com/aceyoga/Sysprog-Log/blob/master/week-12/bootseq.jpg)

In the above picture, the Boot sequence starts from the moment the power button is pressed. Then, the BIOS executes and perform a system checkup(Checking hardware health, temperature, condition, etc) before continuing to the next step. After finishing system checkup, BIOS executes the MBR(Master Boot Record, the first 512 bytes on disk). The MBR loads the first 512 bytes(GRUB stage 1), then the first 30KB of the disk which contains the file system driver(GRUB stage 1,5), and then GRUB loads the kernel selection menu from menu.lst. At this point, the user can select which kernel to use(if there are more than 1 kernel inside /boot/).

After the kernel image is loaded, GRUB loads its initial RAM disk, and if the kernel isn't compiled with the required drivers it will try to find all needed drivers from driver collections stored in RAM disk, in which the file is named initrd/initramfs. Some of the modules stored inside is filesystem, harddisk, network adapters, input devices, etc.

When the kernel has every resource needed to start the complete OS, it mounts the ROOT filesystem to /, then it executes init on /. The init process then starts running everything written in a Startup/Boot Script(Running daemons, background services, preparing drivers, etc).

Finally, after everything is done, user can now login and starts using the machine.

_____

## Customizable Startup Script

_____

The Startup Script is a special script used to "Configure" everything the kernel need to do before finishing the boot process. We can do many thing with this, some examples are:

- Setting up processor frequencies(overclocking/underclocking)
- Set kernel voltages
- Set ondemand scheduler parameters
- Start system/user made daemons(cron, apache, sshd, systemd, etc)
- Load additional kernel modules
- Turn on additional swapping partitions
- Mount additional partitions(/home, /usr, /var, /mount, /etc, etc)
- Run custom made programs
- Etc

Before tempering with a Startup Script, it's best to do a backup beforehand, to prevent unwanted consequences(kernel failing to load, panicking, etc).

## BIOS and MBR vs UEFI and GPT

_____

BIOS is shorthand of Basic Input Output System. A low-level software used as the proven software for starting up booting sequence. The BIOS runs a Power-On Self Test upon machine power-on, checking every hardware is healthly, working properly, and is correctly configured. After checking everything and makes sure it's right, BIOS looks for a Master Boot Record and used it to run the bootloader of the machine's currently installed OS.

The UEFI(Unified Extensible Firmware Interface) is the replacement software to the BIOS. It's not limited by BIOS limitations, like 16-bit processor mode requirement, 1MB of execution space, etc. The UEFI standard can boot from drives with sizes larger than 2.2 TB, with the theoretical limit reaching 9.4 zettabytes. This feat is achieved by the use of GPT(GUID Partition Table) Partitioning Scheme(Instead of BIOS's MBR partitioning scheme). UEFI can run in 32-bit/64-bit processor mode and has faster boot process time than BIOS. Some UEFI screens has a interactable GUI, even if most PC/Laptop still have text-based one.

In short, UEFI and GPT will replace BIOS and MBR with all of it's advantages and features, and soon BIOS and MBR will become a legacy software.

_____

## The Init and Systemd

_____

The init is the "First processes" which also means that every other process is a child of init. Its main task is to execute processes listed in /etc/inittab, like daemons, background processes, and user-level services. Init also controls autonomous processes required by any particular systems. Init is first invoked as the last step of a boot sequence.

The systemd is a software suite, which provides vast array of system components for Linux OS. It's main component is a "System and Service Manager", which is an extended init program, with various daemons, utilities and services ready to use.

_____

## Kernel 101

_____

The Linux Kernel is a Free and Open-Source, Monolithic, Modular, Multitasking, UNIX-Like OS Kernel. This means that:

- Everyone can create, write, configure and compile their own Linux Kernel
- The entire OS works in kernel space
- Insert/Remove additional kernel modules on runtime
- Parallel Computing & Execution
- Is compatible with most machines, usually those already running Linux/other UNIX-based OS.

A Kernel can be compiled with a minimum of critical modules(hardware drivers, input drivers, and other system drivers only) or with many extensive modules(advanced hardware like Joystick, Camera, etc) by configuring the .config file, either manually or via make menuconfig(or other commands).

_____

## Kernel Compiling

_____

So, how do we compile a Kernel? The steps are:

1. Download the Kernel base at kernel.org or kambing.ui.ac.id/linux.

2. Make the configuration file(choose modules to include and exclude, etc).

   - An easy way to do this is to copy the config files used by the currently running kernel from 

     ```
     /boot/config-<kernel version>
     ```

     and renaming it to .config

   - Then we can directly run make menuconfig and select the modules we want to include/exclude.

3. Compile the Kernel

   - The time it takes to compile a kernel ranges from 1-4 hours, or maybe more, depending on system performance(CPU frequency, number of cores used, etc).
     - In my case, it ranges from 1-3 hours(The fastest is around 64 minutes). Proof:

   ![proof](https://github.com/aceyoga/Sysprog-Log/blob/master/week-12/proof.jpg)

   - It will first compile the kernel to its binary compiled form, and then its modules.

   - In debian, we can use make-kpkg to automate the compiling process and make it easier for us to compile(make sure to install all of its dependency before using). The command used is:

     ```bash
     make-kpkg --initrd --append-to-version=<your-version-name> kernel_image kernel_headers
     ```

4. Install the kernel and its modules.

   - Use make install and make modules_install to install the kernel and its modules respectively. In debian, we can use dpkg to install it. The command for debian is:

     ```bash
     dpkg -i kernel_image-<your-version-name>-i386.deb
     dpkg -i kernel_headers-<your-version-name>-i386.deb
     ```

   - After installing, we need to update the bootloader and it's supporting driver files. This means that we need to update the initramfs and the GRUB bootloader. The command is:

     ```bash
     update-initramfs -c -k <kernel version>
     update-grub
     ```

   - After updating, the kernel should show up in the GRUB Kernel Selection Menu.

5. Restart the machine and test out the newly compiled kernel.

   - At the GRUB bootloader, choose the advanced option, then select the kernel that you have just compiled and installed.

   - After you logged in(The username & password is usually the same since the disk partition stores the same data for all kernels), you can check whether the kernel is yours by using uname -a. It will show something like this:

     ![unamea](https://github.com/aceyoga/Sysprog-Log/blob/master/week-12/unamea.jpg)

     It will show the machine name, kernel version, date time, etc.

_____

## References

_____

1. Systems Programming Learning Material Slide 13-Boot Sequence & Kernel Compilation
2. [What Is UEFI, and How Is It Different from BIOS? (howtogeek.com)](https://www.howtogeek.com/56958/HTG-EXPLAINS-HOW-UEFI-WILL-REPLACE-THE-BIOS/)
3. [What’s the Difference Between GPT and MBR When Partitioning a Drive? (howtogeek.com)](https://www.howtogeek.com/193669/whats-the-difference-between-gpt-and-mbr-when-partitioning-a-drive/)
4. [init - Unix, Linux Command - Tutorialspoint](https://www.tutorialspoint.com/unix_commands/init.htm)
5. [systemd - Wikipedia](https://en.wikipedia.org/wiki/Systemd)
6. [Linux kernel - Wikipedia](https://en.wikipedia.org/wiki/Linux_kernel)
7. [Monolithic kernel - Wikipedia](https://en.wikipedia.org/wiki/Monolithic_kernel)
8. man make
9. man uname
10. man make-kpkg
11. man time

