---
layout: post
title: Nathan Android Emulator for Mobile Security Testing Tool
category: Security
tags: security
---

### Introduce

 Nathan is a 5.1.1 SDK 22 AOSP Android emulator customized to perform mobile security assessment.

### Supported architectures:

1. x86
2. arm (soon)

***The emulator is equipped with the Xposed Framework and the following pre-installed modules:***

- SSLUnpinning, to bypass SSL Certificate pinning.

- Inspeckage, to perform the dynamic analysis of an application.
- RootCloak, to bypass root detection.

***The following tools are already installed:***

- SuperSU: Superuser access management tool
- Drozer: Comprehensive security and attack framework for Android

### Features


1. Only python 2.7.x required
2. Hooking ready with Xposed
3. Pre-installed tools for application analysis
4. Fully customizable
5. Snapshot and restore of user data


### Installation


Download Nathan core scripts from git:

    $ git clone https://github.com/mseclab/nathan/
    $ cd nathan

Init Nathan for the first time (for downloading firmware files)

    $ ./nathan.py init

If a proxy is required to download files, the parameter -dp is available :

    $ ./nathan.py init -dp 127.0.0.1:3128

The init command downloads all the files required to run use Nathan Emulator.

### Usage


***To start Nathan:***

    $ ./nathan.py start

***To redirect the traffic through a proxy (es. http://127.0.0.1:3128), the parameter -p can be used:***

    $ ./nathan.py start -p http://127.0.0.1:3128

***To create a snapshot of current user image data with a label (current in this case):***

    $ ./nathan.py snapshot -sl current

***To restore the emulator to the snapshot with label current:***

    $ ./nathan.py restore --rl current

***To get a list of available snapshots to restore:***

    $ ./nathan.py restore --ll  

**Every time the emulator is started, a temporary copy of system image is created and each changes made to system data is lost when the emulator is powered off.

To keep permanent the changes, the command freeze is available:**

    $ ./nathan.py freeze  

***To push files from a folder to a running Nathan emulator, the command push is available:***

    $ ./nathan.py push -f folder  

***The complete list of command is:***

```bash
usage: nathan.py [-h] [-v] [-a ARCH]
{init,start,snapshot,restore,freeze,push} ...

Optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         Show emulator/kernel logs
  -a ARCH, --arch ARCH  Select architecture (arm/x86) - Default = x86

Command to run:
         {init,start,snapshot,restore,freeze,push}
    init                Download and init Nathan emulator
    start               Start Nathan emulator
    snapshot            Create userdata image snapshot
    restore             Restore userdata image snapshot
    freeze              Freeze temporary system image
    push                Push files to Nathan emulator
```

The parameter -h for each command shows specific options.
