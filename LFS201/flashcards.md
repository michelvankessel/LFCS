## Flashcards LFS201
### Based on Knowledge Checks at the End of each Chapter

### 2 - Linux Filesystem Tree Layout
* Used System Wide Config Files - /etc
* Contains individual user data - /home
* Contains system log files     - /var
* Early system startup files    - /boot
* Contain linux kernel modules  - /lib
* Abstract file systems         - /dev & /proc & /sys & /run

### 3 - Processes

* Used to change max # of open file to 2048 - ulimit -n 2048
* Set niceness to 5 units of process with PID 444 - renice (+)5 444
* A process in zombie state - has terminated but no other process has asked about its exit state

### 4 - Signals

* kill, killall, pkill all accept process name as parameter - False
* Passing -SIGKILL & -9 have the same effect on target process - True
* If not specified, kill will send SIGTERM by default - True
* Pressing Ctrl-Z or using kill -SIGTSTP have same effects - True

### 5 - Package Management Systems

* Packaging system used by RHEL, SUSE, CentOS & Fedora: rpm
* Git was originally created by Linus Torvalds.

### 6 - RPM

* rpm -qa lists all installed packages on the system - True
* rpm -V coreutils verifies the integrity of the package /bin/ls which is
provided by the coreutils package - True

### 7 -DPKG

* dpkg -l lists all installed packages on the system - True

### 8 - YUM

* `yum install package` is used to install a new package - True
* `yum update` does not accept a package as argument - False
* `yum provides file-path` cannot be used to find which package provides the file specified as the argument - False
* `yum search` can be used for searching on package name and short description - True

### 9 - ZYPPER

* `zypper install package` is used to install a new package - True
* `zypper update` doesn't accept a package as argument - False
* `zypper what-provides <file-path>` cannot be used to find which package provides the file specified in the argument - False
* `zypper search` can be used for searching on package name and short description - True

### 10 - APT

* `apt install <package>` is used to install a new package - True
* `apt update` does not accpet a package as argument - True
* `apt-file find <file_path>` can't be used to find which package provides the file specified as argument - False
* `apt-cache search` can be used for searching on package name and short description - True

### 11 - SYSTEM MONITORING

* A tracing & debugging tool that shows how a process make requests to the OS - `strace`
* A tool that shows how long the system is running - `uptime`
* An interactive tool for system monitoring - `top`
* A tool that displays a summary of memory usage - `free`
* What information that is not related to processes can be found at /proc? - All of the following: The Kernel command line, CPU model information, Memory Utilization Statistics, Disk Partition Information

### 12 - PROCESS MONITORING

* ps, top, pstree are system monitoring tools
* ps -elf shows the PPIDs for all the processes on the system

### 13 - MEMORY MONITORING AND USAGE

* Disabling all swap partitions increases the chances of the sytem invoking the OOM-killer - True
* Memory used for file caching has more priority than memory used by system processes. The OOM-killer is invoked if there is insufficient memory for both file caching and the user process. - False
* What is moved to the swap area is never moved back to the main memory. - False
* The field **MemFree** entry in `/proc/meminfo` shows the amt of memory that is not utilized, but the file caching could also be considered free memory as the kernel would release memory used by the cache if applications need more memory. - True

### 14 - I/O MONITORING and TUNING

* ionice is used to configure the I/O priority of a process
* `sudo iotop -o` will continuously show activity for all tasks currently doing I/O

### 15 - I/O SCHEDULING


