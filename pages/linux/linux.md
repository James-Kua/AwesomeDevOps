---
title: "Linux"
---
# Linux

![](https://www.brendangregg.com/Perf/linux_perf_tools_full.png)

## Files

### lsblk

lsblk lists information about all or the specified block devices. The lsblk command reads the sysfs filesystem to gather information.

The command prints all block devices (except RAM disks) in a tree-like format by default. Use lsblk --help to get a list of all available columns.

```bash
$lsblk
NAME                         MAJ:MIN RM   SIZE RO TYPE   MOUNTPOINT
sda                            8:0    0 931.5G  0 disk
├─sda1                         8:1    0   500M  0 part   /boot
└─sda2                         8:2    0   931G  0 part
  ├─vg_xldesk-lv_root (dm-0) 253:0    0    50G  0 lvm    /
  ├─vg_xldesk-lv_swap (dm-1) 253:1    0  17.7G  0 lvm    [SWAP]
  └─vg_xldesk-lv_home (dm-2) 253:2    0   1.8T  0 lvm    /home
sdc                            8:32   0 232.9G  0 disk
└─sdc1                         8:33   0 232.9G  0 part
  └─md1                        9:1    0 232.9G  0 raid10 /data
sdb                            8:16   0 931.5G  0 disk
└─sdb1                         8:17   0 931.5G  0 part
  └─vg_xldesk-lv_home (dm-2) 253:2    0   1.8T  0 lvm    /home
sdd                            8:48   0 232.9G  0 disk
└─sdd1                         8:49   0 232.9G  0 part
  └─md1                        9:1    0 232.9G  0 raid10 /data
sr0                           11:0    1  1024M  0 rom
```

## Disk

### df
The df command displays disk space usage for all mounted file systems. It shows information such as total disk space, used space, available space, and the file system type.

- Display all filesystems and their disk usage using 512-byte units:
    df

- Use [h]uman-readable units (based on powers of 1024) and display a grand total:
    df -h -c

- Use [H]uman-readable units (based on powers of 1000):
    df --si|H

- Display the filesystem and its disk usage containing the given file or directory:
    df path/to/file_or_directory

- Include statistics on the number of free and used [i]nodes including the filesystem t[Y]pes:
    df -iY

- Use 1024-byte units when writing space figures:
    df -k

- Display information in a [P]ortable way:
    df -P

### du
The du command is used to estimate file space usage. It displays the disk space occupied by files and directories, either in total or recursively for directories.

- List the sizes of a directory and any subdirectories, in the given unit (KiB/MiB/GiB):
    du -k|m|g path/to/directory

- List the sizes of a directory and any subdirectories, in human-readable form (i.e. auto-selecting the appropriate unit for each size):
    du -h path/to/directory

- Show the size of a single directory, in human-readable units:
    du -sh path/to/directory

- List the human-readable sizes of a directory and of all the files and directories within it:
    du -ah path/to/directory

- List the human-readable sizes of a directory and any subdirectories, up to N levels deep:
    du -h -d 2 path/to/directory

- List the human-readable size of all `.jpg` files in subdirectories of the current directory, and show a cumulative total at the end:
    du -ch */*.jpg

### fdisk
The fdisk command is a partition table manipulator for Linux. It allows you to create, modify, and delete disk partitions on storage devices.

### mkfs
The mkfs command is used to create file systems on storage devices. It formats partitions with a specified file system type, such as ext4 or xfs.

### mount
The mount command is used to mount file systems onto the Linux directory tree. It establishes a connection between the file system and the operating system, allowing access to the files and directories stored on the mounted device.

- Show all mounted filesystems:
    mount

- Mount a device to a directory:
    mount -t filesystem_type path/to/device_file path/to/target_directory

- Create a specific directory if it does not exist and mount a device to it:
    mount --mkdir path/to/device_file path/to/target_directory

- Mount a device to a directory for a specific user:
    mount -o uid=user_id,gid=group_id path/to/device_file path/to/target_directory

- Mount a CD-ROM device (with the filetype ISO9660) to `/cdrom` (readonly):
    mount -t iso9660 -o ro /dev/cdrom /cdrom

- Mount all the filesystem defined in `/etc/fstab`:
    mount -a

- Mount a specific filesystem described in `/etc/fstab` (e.g. `/dev/sda1 /my_drive ext2 defaults 0 2`):
    mount /my_drive

- Mount a directory to another directory:
    mount --bind path/to/old_dir path/to/new_dir

### umount
The umount command is used to unmount file systems from the Linux directory tree. It detaches the file system from the operating system, allowing the device to be safely removed or unmounted.

### lvm
The Logical Volume Manager (LVM) provides tools and commands for managing logical volumes and volume groups in Linux. It allows for dynamic volume management, including resizing logical volumes and adding or removing physical volumes.

### resize2fs
The resize2fs command is used to resize ext2, ext3, and ext4 file systems. It allows you to expand or shrink the size of a file system on a disk partition.

### parted
The parted command is a partition manipulation program that supports various disk partitioning formats. It allows you to create, resize, and manipulate disk partitions with a user-friendly interface.

### gdisk
The gdisk command is a disk partitioning tool similar to fdisk, but it supports GUID Partition Table (GPT) disks. It allows for creating, modifying, and deleting GPT partitions on storage devices.

### smartctl
The smartctl command is used to monitor and control Self-Monitoring, Analysis, and Reporting Technology (SMART) enabled devices. It provides information about disk health, temperature, and error rates.

## System Management

### top
The top command provides real-time information about system processes, CPU usage, memory usage, and other system metrics. It's useful for monitoring system performance and identifying resource-intensive processes.

- Start `top`, all options are available in the interface:
    top

- Start `top` sorting processes by internal memory size (default order - process ID):
    top -o mem

- Start `top` sorting processes first by CPU, then by running time:
    top -o cpu -O time

- Start `top` displaying only processes owned by given user:
    top -user user_name

### ps
The ps command displays information about active processes on the system. It can be used to list all processes, filter processes by criteria such as user or process ID, and display detailed information about specific processes.

- List all running processes:
    ps aux

- List all running processes including the full command string:
    ps auxww

- Search for a process that matches a string:
    ps aux | grep string

- Get the parent PID of a process:
    ps -o ppid= -p pid

- Sort processes by memory usage:
    ps -m

- Sort processes by CPU usage:
    ps -r


### lsof

Lists open files and the corresponding processes.

- Find the processes that have a given file open:
    lsof path/to/file

- Find the process that opened a local internet port:
    lsof -i :port

- Only output the process ID (PID):
    lsof -t path/to/file

- List files opened by the given user:
    lsof -u username

- List files opened by the given command or process:
    lsof -c process_or_command_name

- List files opened by a specific process, given its PID:
    lsof -p PID

- List open files in a directory:
    lsof +D path/to/directory

- Find the process that is listening on a local IPv6 TCP port and don't convert network or port numbers:
    lsof -i6TCP:port -sTCP:LISTEN -n -P

### free

The free command displays information about system memory usage, including total memory, used memory, free memory, and memory used for buffers and caches. It's helpful for monitoring memory usage and identifying memory-related issues.

## Networking

### ifconfig / ip
ifconfig is a command-line utility used to configure and display information about network interfaces on the system. However, ip is the modern replacement for ifconfig and provides more advanced functionality for configuring network interfaces, routes, and other networking parameters.

- List all network interfaces:
    ipconfig getiflist

- Show the IP address of an interface:
    ipconfig getifaddr interface_name

### netstat
The netstat command displays information about network connections, routing tables, interface statistics, and other network-related information. It's useful for troubleshooting network issues and monitoring network activity.

### ss
Similar to netstat, the ss command is used to display information about network sockets and connections. It provides more detailed and up-to-date information compared to netstat and is preferred for modern systems.
