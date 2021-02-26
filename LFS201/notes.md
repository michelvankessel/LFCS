## Notes LFS201
#### Anvesh G. Jhuboo

#### 2 - Linux Filesystem Tree Layout
Data Distinctions:
* Shareable vs Non-shareable
* Variable vs Static

Main Directories:
* /         - Primary directory of entire FHS
* /bin      - Essential exec programs that must be available in single user mode
* /boot     - Files needed to boot system like kernel, initrd, boot config, ...
* /dev      - Device Nodes, to interact with Hardware & Software Devices
* /home     - User home directories for personal settings, files, ...
* /lib      - Libraries required by exec binaries in /bin & /sbin
* /lib64    - 64-bit libraries
* /media    - Mount points for removable media, like USBs, CDs, ...
* /mnt      - Temporarilty Mounted Filesystems
* /opt      - Optional application software packages, like Chrome, ...
* /proc     - Virtual pseudo-filesytem giving system info & processes info
* /sys      - Virtual pseudo-filesystem... & smiliar to device tree & part of UDM
* /root     - Home directory of root user
* /sbin     - Essential system binaries
* /srv      - Site-specific data served up by the system. Seldom used
* /tmp      - Temporary files, generally lost across reboot
* /usr      - Multi-user applications, utilities & data. Theoritically Read-only
* /var      - Variable data that changes during system operation
