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
