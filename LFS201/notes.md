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

RPM advantages:
* Makes software packages easy to manage for Sysadmins
    * Easy to determine which pkg a particular file is coming from
    which version of pkg is installed, whether it is correctly installed or not
    * Easy to remove pkgs to free up disk space
    * Distinguishes documentation files from rest of pkg, giving the choice
    to install documentation on the system
* Makes software developer's job easier
    * Allows builders to keep changes necessary for building on Linux separate
    from the original source. This facilitates the incorporation of new versions
    of the code as build-related changes are all in one place.
    * Facilities building versions of Linux for different architectures.

Naming Conventions:
* Binary package: <name>-<version>-<release>.<distro>.<architecture>.rpm
* Source package: <name>-<version>-<release>.<distro>.src.rpm

Query, verify, install, uninstall, upgrade & freshen packages
`var/lib/rpm` - default dir that holds RPM database files
* Query: `rpm -qi bash` Other flags include `-qfl -qa --requires --whatprovides`
* Verify: `rpm -V bash`
* Install: `sudo rpm -ivh bash-5.1.19-8.e18_0.x86_64`
* Uninstall: `sudo rpm -e bash`
* Upgrade: `rpm -Uvh bash`
* Freshen: `sudo rpm -Fvh *.rpm`

New kernels should be installed rather than upgraded, because if ever there are
any problems, you can still boot in the old kernel. Once the new kernel has been
tested, once then you can remove the old kernel. Unless you are short on space,
it is recommended to keep one or more older kernels for safety.

rpm2cipo converts or extracts RPM package files to cpio archives
* `rpm2cpio foobar.rpm > foobar.cpio`
* `rpm2cpio bash-XXX.rpm | cpio -ivd bin/bash`


### 7 -DPKG

DPKG advantages & Uses
* Same as RPM advantages, see above

Naming conventions
* Binary: <name>-<version>-<revision-number>-<architecture>.deb
* Source: see below
    * use `apt-get source <program_name>` to download source
    * use `du -shc <program_name>*` to view all files downloaded from source

Know what Source packages look like
Source package consists of at least 3 files:
* Unmodified source code: Upstream tarball ending in `.tar.gz`
* Description file ending in `.dsc`
* Second tarball containing patches & additional files ending in `.diff.gz`
or `debian.tar.gz`

Query & Verify operations on packages
* `dpkg -I <pkg_file>` - show info about pkg file
* `dpkg -V pkg` - verify pkg

Install, upgrade, uninstall Debian packages
* `sudo dpkg -i foobar.deb` - install or upgrade
* `sudo dpkg -r package`    - remove
* `sudo dpkg -P package`    - purge, includes config files


### 8 - YUM

`yum` provides a frontend to `rpm`. Its main task is to fetch packages from 
multiple remote repos and resolve dependencies among packages. It also caches 
infomation and databases to speed up performance.

Repository config files for `yum` are kept in `/etc/yum.repos.d` and have a
`.repo` extension. The use of a particular repo can be toggled on or off by 
changing the value off enabled to 0 or 1, or using `--disablerepo=somerepo` and
`--enablerepo=somerepo` when using yum.

Queries
* `yum search keyword`
* `yum list "*keyword*"`
* `yum info package-name`
* `yum grouplist`
* `yum groupinfo package-group`
* `yum provides /path/to/file`

Additonal
* `yum verify [package]`
* `yum install package1 [packages]`
* `yum remove package1 [packages]`
* `yum update pakage`

To install a new repo, say for example webmin:
* Go to /etc/yum.repos.d
* Create a repo file, named webmin.repo, containing the following main things
```
[Webmin]
name=Webmin Distribution Neutral
baseurl=http://download.webmin.com/download/yum
mirrorlist=http://download.webmin.com/download/yum/mirrorlist
enabled=1
gpgcheck=0
```
* install the webmin package using `sudo yum install webmin`

`dnf` is now replacing `yum`.


### 9 - ZYPPER

`zypper` is a high level command line tool for installing and managing packages
on SUSE Linux and openSUSE. It is very similar to `yum`, and also works with
`rpm` packages.

Queries
* `zypper list-updates`
* `zypper repos`
* `zypper search <string>`
* `zypper info <package>`
* `zypper search --provides /path/to/file`

Install, Remove, Upgrade
* `sudo zypper install <package>`
* `sudo zypper update <optional-packages>`
* `sudo zypper remove <package>`

Advanced
* `sudo zypper shell` allows a number of zypper commands to run in sequence.
This has the added advantage that the databases are not re-read for each command.
* `sudo zypper addrepo URI alias` - add a new repository, located at the supplied URI, and which will use supplied alias
* `sudo zypper removerepo alias` - remove repository
* `sudo zypper clean [-all]` - clean up and save space in `/var/cache/zypp`


### 10 - APT

APT is used on Debian systems, it provides a higher level of intelligent services
that use the underlying dpkg program. It plays the same role as `yum` and `zypper`
on Red Hat based systems. It works with Debian packages that have the `.deb`
extension.

It should be noted that `apt` and `apt-get` are now equivalent.

Queries
* `apt-cache search package`
* `apt-cache show package`  - list basic info about package
* `apt-cache showpkg package` - list detailed info about package
* `apt-cache depends package` - list dependent packages for package
* `apt-cache rdepends package` - list packages package depends on
* `apt-file list package` - list all files in package

Install, Remove, Upgrade
* `apt update` synchronize package index files with their repo sources
* `apt install [package]` - install or update already installed package
* `apt remove [package]`
* `apt --purge remove [package]` - remove package and its config files
* `apt upgrade` - apply updates to packages already installed
* `apt dist-upgrade` - do smart upgrade, with more thorough dependency resolution
and remove obsolete packages, and install new dependencies
* `apt autoremove` - get rid of packages not needed anymore
* `apt clean` - clean cache files and any archived package files that have been installed


### 11 - SYSTEM MONITORING

Process & Load Monitoring Utilities
Utility | Purpose
------- | ------- 
top     | Process activity, dynamically updated
uptime  | How long system is running & average load
ps      | Detailed info about processes
pstree  | A tree of processes & their connections
mpstat  | Multiple processor usage
iostat  | CPU utilization & I/O statistics
sar     | Display and collect information about system activity
numastat| Information about NUMA (Non-Uniform Memory Architecture)
strace  | Information about all system calls a process makes

Memory Monitoring Utilities
Utility | Purpose
------- | -------
free    | Brief summary abt memory usage
vmstat  | Detailed vm statistics & block I/O, dynamically updated
pmap    | Process memory map

I/O Monitoring Utilities
Utility | Purpose
------- | -------
iostat  | CPU utilization & I/O statistics
sar     | Display & collect info about a system activity
vmstat  | Detailed vm statistics & block I/O, dynamically updated

Network Monitoring Utilities
Utility | Purpose
------- | -------
netstat | Detailed networking statistics
iptraf  | Gather information on network interfaces
tcpdump | Detailed analysis of network packets & traffic
wireshark| Detailed network traffic analysis


LOG FILES  
System log files are stored under `/var/log`  
Use `sudo tail -f /var/log/messages` or `/var/log/syslog` to view new msgs  
Or `dmesg -w` to view only kernel-related msgs   

Some important log files
File                | Purpose
----                | -------
boot.log            | System boot messages
dmesg               | Kernel msgs saved after boot.  
messages or syslog  | All important system msgs
secure              | Security-related msgs

`logrotate` is used to prevent log files from growing out of bounds.


PROC & SYS pseudo-filesystems  
Most turnable system parameters can be found under `/proc/sys`  
Viewing and changing parameters:  
`$ cat /proc/sys/kernel/threads-max`  
`$ sudo bash -c 'echo 100000 > /proc/sys/kernel/threads-max'`  
`$ sudo sysctl kernel.threads-max=100000`  
The /sys pseudofilesystem is more tightly defined that /proc. Most entries contain only one line of text.

`sar` stands for Systems Activity Reporter. It is an all purpose tool for
gathering system activity and performance data and creating reports that are
readable by humans. The backend to sar is `sadc` (system activity data collector).

`$ sar [options] [interval] [count]`


### 12 - PROCESS MONITORING

Use ps to view characteristics and statistics associated with processes
`ps` is used to view characteristics and statistics associated with processes,
all of which are generated by the /proc directory associated with the process.

The ps output can be customized as such:  
`$ ps -o ppid,pid,uid,cputime,pmem,command,time,state`

Some common selection options in the UNIX format are:
* -A or -e : select all processes
* -N : negate selection
* -C : select by command name
* -G : select by group ID or name
* -U : select by real User ID or name

`pstree` is used to give a visual description of process ancestry and
multi-threaded applications

`top`  is used to view system loads interactively.


### 13 - MEMORY MONITORING & USAGE

st the primary considerations and tasks involved in memeory tuinign

Use entris in /rpoc/ss/vm and decipher /rpoco/meminfo

Use vm stat to dispaly information about memory, paign, I/O, processor activit  ad process' memory conumtpiotn

Userstand how the OOM-hiller decides when to tkae action and seelcts which processes showuls be exterminated to open up som ememory


