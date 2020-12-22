# System Programming Logbook Week  14

Muhamad Yoga Mahendra |1806205256 | System Programming - B

## Table of Content

- [Commands & Programs for Modules](#Commands_&_Programs_for_Modules)
- [Loadable Kernel Modules](#Loadable_Kernel_Modules)
- [Audio Devices and Drivers](#Audio_Devices_and_Drivers)
- [Advanced Linux Sound Architecture](#Advanced_Linux_Sound_Architecture)
- [My Audio Devices, Drivers, and ALSA Setup](#My_Audio_Devices,_Drivers,_and_ALSA_Setup)
- [References](#References)

![nama gambar](https://github.com/aceyoga/Sysprog-Log/blob/master/week-10/namafile.tipe)



_________________________________________________________________________________________________________________________________________________________________________

## Loadable Kernel Modules

_____

Loadable Kernel Modules is a module program that can be easily loaded/unloaded to a kernel at runtime. 

_________________________________________________________________________________________________________________________________________________________________________

## Commands & Programs for Modules

_____

In the process of creating kernel modules and device drivers, there are many tools used by developers. Ranging from text editor programs(vi, vim, nvim, nano, etc), to the driver installer(like dpkg). This section will briefly explain tools and commands that I used to create, and install kernel modules & device drivers.

- Text Editor nano
  - According to its man page, nano(Nano's ANOther editor) is a small and friendly editor. It copies the look and feel of Pico, but nano is a free software and also have features that the pico doesn't have. 
  - How to use: 
    - nano filename
    - After the editor is shown, we can directly start editing the file, instead of manually entering insert mode like vi does. To exit, press CTRL + X, then y(to save data written to buffer to file), then enter(to save to the designated filename).
- The make command
  - Make is a GNU utility used to maintain groups of programs. It can be used to set up environment, get dependencies, compile programs, install programs and even do a cleanup.
  - Before using make, we need to create a Makefile first, where the contents is used to describe relationships among the files in the group, and the commands used to perform wanted tasks.
  - After creating a suitable Makefile, the simple use of "make" will do everything specified in the Makefile.
  - To make a module, first we need to specify the modules to be compiled, the kernel used to compile, and other dependencies(if any) in the Makefile. Then we run make.
- The apt-get command
  - It is a package manager utility, used to handle the process of installing packages, dependencies, and managing installed packages. Some important commands for the apt-get utility is:
    - update, this command resyncs the package index files from their respective sources.
    - upgrade, this command installs the newest package of all existing packages in the system by checking the source listed in the package index files.
    - install, this command installs the specified packages, along with its dependencies(if any).
    - remove, this command removes the specified packages, but not removing its dependencies.
- The dpkg and dkms command
  - The dpkg is a special package manager intended for Debian Linux distributions, to manage(install, build, remove, configure) Debian packages.
  - The dkms(Dynamic Kernel Module Support) is a framework that allows a kernel modules to be dynamically built for each kernel existing in a system in a simplified and organized fashion. 

_____

## Audio Devices and Drivers

_____

In linux, audio drivers are usually named "snd" which is an abbreviation for "sound". In my machine, there are many sound drivers which uses the snd driver as it's dependency. For example, the snd_hda_intel drivers. Proof:

![proof 1](https://github.com/aceyoga/Sysprog-Log/blob/master/week-14/proof1.png)

_____

## Advanced Linux Sound Architecture

_____

The Advanced Linux Sound Architecture(ALSA) is a software framework that provides an interface in the form of device drivers, which enables audio and MIDI functionality to the Linux OS and its distributions. ALSA has the following significant features:

- Efficient support for all types of audio interfaces, from customer(End-User) sound cards to professional multichannel audio interfaces.
- Fully modularized sound drivers.
- SMP and thread-safe design.
- An User-space function library(alsa-lib) to simplify application programming and provide high level functionality to users and programs in the user space.
- Provides support for the older Open Sound System(OSS) API, providing binary compatibility for most OSS programs.

To install ALSA in a linux system, simply run the package manager(apt-get in this example) along with it's packages:

sudo apt-get install alsa alsa-base alsa-utils alsa-tools libasound2

To use it's driver interface(example: to increase/decrease sound volume) we can use the alsamixer program. The alsamixer is a soundcard mixer for ALSA soundcard drivers, and it uses ncurses interface as it's UI. It supports multiple soundcards with multiple devices.

In my machine, it looks like this:

![alsamixer](https://github.com/aceyoga/Sysprog-Log/blob/master/week-14/alsamixer1.png)

In order to use the microphone, we can use the arecord program to record the microphone input. To play sound files(WAV files, etc) we can use aplay program. To use both programs, just type the program name followed by the target filename and it's parameter settings.

_____

## My Audio Devices, Drivers, and ALSA Setup

_____

My Linux system using the sysprog-ova image, has the following setup:

- Sound Card: HDA Intel
- Sound Chip: SigmaTel STAC9221 A1
- Sound Capture devices:
  - By using arecord -l, the output is:

```
**** List of CAPTURE Hardware Devices ****
card 0: Intel [HDA Intel], device 0: STAC9221 A1 Analog [STAC9221 A1 Analog]
  Subdevices: 0/1
  Subdevice #0: subdevice #0
card 0: Intel [HDA Intel], device 1: STAC9221 A1 Digital [STAC9221 A1 Digital]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: Intel [HDA Intel], device 2: STAC9221 A1 Alt Analog [STAC9221 A1 Alt Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

- Device Drivers:
  - snd, snd_hda_intel, etc.

_____

## References

_____

1. Systems Programming Learning Material Slide 21 - Writing Drivers
2. https://en.wikipedia.org/wiki/Loadable_kernel_module
3. man nano
4. man make
5. man apt-get
6. man dpkg
7. man dkms
8. https://www.alsa-project.org/wiki/Main_Page
9. https://en.wikipedia.org/wiki/Advanced_Linux_Sound_Architecture
10. http://howto.blbosti.com/2010/03/ubuntu-server-install-alsa-sound-and-moc-music-on-console/
11. https://wiki.ubuntu.com/Audio/UpgradingAlsa/DKMS
12. https://www.kernel.org/doc/html/latest/sound/index.html
13. https://www.kernel.org/doc/html/latest/sound/kernel-api/index.html
14. https://www.kernel.org/doc/html/latest/sound/hd-audio/models.html#stac9220-9221
15. man alsamixer
16. man arecord
17. man aplay

