## Notes LFS201
### Learning Objectives Answered
#### Anvesh G. Jhuboo

### 2 - Linux Filesystem Tree Layout
Linux requires the organization of one big filesystem tree as this standardizes
the various distinct filesystems, and makes it easier for developing applications
and accomplishing system adminstration tasks.

The Filesystem Hierarchy Standard specifies the main directories that need to be
present and describes their purposes. This simplifies predictions of file 
locations.

At boot, the following must be available in the root(/) directory: /bin, /boot,
/lib, /sbin

Only once the system is started are the following directories available: /dev,
/etc, /home, /mnt, /opt, /proc, /sys, /srv, /tmp, /usr, /var


Data Distinctions:
* Shareable vs Non-shareable
* Variable vs Static

Main Directories:
Directory | Purpose
--------- | -------
/         | Primary directory of entire FHS
/bin      | Essential exec programs that must be available in single user mode
/boot     | Files needed to boot system like kernel, initrd, boot config, ...
/dev      | Device Nodes, to interact with Hardware & Software Devices
/etc      | Machine local config files & some startup scripts
/home     | User home directories for personal settings, files, ...
/lib      | Libraries required by exec binaries in /bin & /sbin
/lib64    | 64-bit libraries
/media    | Mount points for removable media, like USBs, CDs, ...
/mnt      | Temporarilty Mounted Filesystems
/opt      | Optional software packages, who wish to isolate all their files
/proc     | Virtual pseudo-filesytem giving system info & processes info
/sys      | Virtual pseudo-filesystem... & smiliar to device tree & part of UDM
/root     | Home directory of root user
/sbin     | Essential system binaries for booting, restoration, recovery, repair
/srv      | Site-specific data served up by the system. Seldom used
/tmp      | Temporary files, generally lost across reboot
/usr      | Multi-user applications, utilities & data. Theoritically Read-only
/var      | Variable data that changes during system operation
/run      | Peusdo-filesystem that stroes transient files (runtime information)


### 3 - Processes

A process is an instance of an executing program, and associated resources. These resources include environment, file handles, signal handlers, allocated memory ...

The init process is the first user process run on a system. It is thus the 
ancestor of all subsequent processes running on the system, except those directly
initiated by the kernel (which show up with [] around their name in ps listing).

A program is a set of instructions along with any internal data used while
carrying the instructions out. The same program may be executing more than once
and hence be responsible for multiple processes.

A thread, or thread of execution, is the smallest unit of processing that a 
scheduler can work on. A process can have multiple threads of execution which are
executed asynchronously. These threads of execution can share various resources,
such as their memory spaces, open files, etc. Each thread returns the same
process ID (thread group ID) while returning a distinct thread ID (process ID).

Process attributes include:
* The program being executed
* Context (state)
    * A snapshot of the process trapping the state of its CPU registers, where it
    is executing the program, what is in the process' memory & other infomation 
    * Context switching: being able to store the entire context when swapping out
    the process, and being able to restore it upon execution redemption
* Permissions
* Associated resources
    * environment, file hangles, signal handlers, allocated memory, ...


Process Permissions:
* Effective User ID (effective UID)
    * when a process is launched, it runs with the same permissions as the user
    or group that ran it. Used to grant access rights to a process.
* Real User ID (real UID)
    * this is the ID of the user that launched the process.
* Saved User ID (saved UID)
    * allows a process to switch between effective UID and real UID
    * use to escalate priviledges (setuid programs)

Process States:
* Running
* Sleeping (Waiting)
* Stopped
* Zombie

Use `$ ulimits` to control limits

User Mode Vs Kernel Mode
* User Mode: Each process is isolated in its own user space to protect it from 
other processes. This process resoucre isolation promotes security & stability. 
* Kernel (System) Mode: CPU has full access to all hardware on system. 
If application needs acces to these resources, it must issue a system call.

A demon process is a background process whose sole purpose is to provide some
specific service to users of the system. Examples include httpd & systemd-udevd.

A linux system is always creating new process - this is called forking, whereby
the parent keeps running, while a new child process starts.
Often rather than fork, one follows it with *exec* where the parent process
terminates, and the child process inherits the process ID of the parent.

nice & renice can be used to set priorities
```
$ nice -5 command [ARGS]
$ renice +5 -p <pid>
```

Static libaries vs Shared Libraries (DLLs)
* Static: code for the library functions is inserted into the program at compile
time and doesn't change thereafter, even if the libary updates. 
* Shared: code is inserted at run time, and if library is changed later, the
running program will run with those libarary modifications. Has '.so' extension.

### 4 - Signals

Signals are used to notify processes about asynchornous events (or exceptions).
* There are two ways in which signals are sent to processes:
    * From the kernel to the user process, as a result of an exception
    or a programming error
    * From a user process using a system call to the kernel which will then send
    it to the user process. 

Using `kill -l` lists all the available signals
The most important ones include
* SIGKILL -9    : Kill signal, cannot be caught or ignored
* SIGTERM -15   : Process termination (default)
* SIGSTOP -19   : Stop process, cannot be caught or ignored

Users can send signals to other processes using `kill <process-id>`

The term kill is a really bad name, because the command real function is to
send any and all signals, not limited to killing, to processes.

killall kills all process with a given name. `killall <process-name>`

pkilll sengs a signal to a process using selection criteria.
`pkill <process-name>`


### 5 - Package Management Systems

Package Management Systems make it easy to automate installing, upgrading,
configuring, and removing software packages in a know, predictable and
consistent manner. They make it easy for installation processes to scale to
thousands of systems without requiring manual work on each individual system.

Binary vs Source Packages
* Binary: contain files ready for deployment, including executables and 
libarries. They are architecture dependent, and must be compiled for each.
* Source: used to generate binary packages. Allows to rebuild a binary package
from the source package. Can be used for many architectures.

Main Two Packaging System
* rpm - used by RHEL, Fedora, Centos, SUSE & related openSUSE distributions
* dpkg - used by Debian-derived distributions, like Debian, Ubuntu, ...

Two level of utilities are available:
* Low level utilities
    * Installs or removes a single package, or a list of packages, each one of
    which is individually & specifically named. Dependencies are not fully
    handles, only warned about. If another package needs to installed, first
    installation will fail. If package is needed by another package, removal
    will fail.
    * Examples are: *rpm & dpkg*
* High level utilities
    * Solves dependency problems.
    * Examples are *apt & apt-cache* for dpkg systems
    * Examples are *yum, dnf, zypper & PackageKit* for rpm systems

Creating your own packages allows you to control what exackly goes in the
software and exactly how it is established. It can be used to perform the
following tasks:
* Creating needed symbolic links
* Setting permissions
* Creating directories as needed
* Anything that can be scripted

Git is a version control system, created by Linus Torvalds & primarily
maintained by Junio C Hamano.


### 6 - RPM

Understand how RPM system is organized and what major operations the rpm
program can accomplish

Explain the naming conventions used for binary & source rpm files

Query, verify, install, uninstall, upgrade & freshen packages

Grasp why new kernels should be insalled, rather than upgraded

Use rpm2cpio to copy packaged files into a cpio archive, as well as extract
the files without installing them
