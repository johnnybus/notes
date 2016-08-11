# Linux Tips
## Common Stuff
###### Edit date or time keeping the previous minutes or year
    date —set="$(date +'%y0207 18:%M:%S')"

##### Use DVD as Repo on yum
###### Create repo file /etc/yum.repos.d/redhat7dvd.repo
    enabled=1
    baseurl=file:///mnt/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

###### List yum repo
    yum repolist

##### Disable/Enable subscription-manager default repo
###### This default redhat.repo repository can be disabled by editing the Subscription-Manager configuration and setting the manage_repos value to zero (0). To enable, set 1.
    subscription-manager config —rhsm.manage_repos=0

##### Recreate /etc/shadow file with pwconv
On the message "passwd: User not known to the underlying authentication module"
    recreate /etc/shadow file with pwconv

##### CentOS Change Resolution (on installation)
###### Method 1
- Edit the virtual machine video card settings and set the video ram to 4MB.
- Boot Centos 7 install disk, hit "TAB" key to change boot options.
- Add the following to the boot line to tell it to use 1024x768x16 resolution.
vga=791

##### Method 2
- Edit the virtual machine video card settings and set the video ram to 4MB.
- Edit the VM settings, click the options tab, click on "boot options" change from BIOS to UEFI.

##### RDP over SSH Tunnel
You don't need a SOCKS proxy for this; simple SSH port forwarding will work. For example, there's a server at my office I frequently need to access, which we'll call server.example.com. I can't connect to it directly, but I can ssh to myofficemachine.example.com.
So I do this: `ssh -L 3389:server.example.com:3389 myofficemachine.example.com`

And then I point my local Remote Desktop client to localhost. This works great, and my setup is almost identical to yours -- a Mac at home, a Linux box at my office, and a Windows server on another work network.

##### NTP, VMware Tools Time
###### Check date and time sync over vmware tools
    vmware-toolbox-cmd timesync status

passwd: User not known to the underlying authentication module
     Recreate /etc/shadow file with pwconv

##### Add Script to Init
###### Copy original script to:
    /etc/init.d/
###### Inside of /etc/rc.d/rc3.d/ (runlevel 3) an symbolic link to your init.d script**
    ln -s /etc/init.d/<script_name> /etc/rc.d/rc3.d/<script_name>

##### Rescue Linux from CD
1. Boot from distribution image  
2. Choose rescue or troubleshooting 
3. Mount OS with write options, the OS normally, will be mounted under `/mnt/sys/`
4. To change the path to OS installed on the HD:
`chroot **&lt;OS mountpoint (/mnt/sys)&gt;**`

## Storage
###### List devices from Multipath
    multipath -ll
###### Location of Multipath configuration file
    /etc/multipath.conf
###### Monitoring LV operations
    watch 'lvs -a -o+devices'
###### List PV’s from a specific VG
    vgs -o vg_name,pv_name,pv_size,pv_free <vgname>

##### Manual HD Scan
###### Each hostX means an scsi or sata controller
    echo "- - -" > /sys/class/scsi_host/host0/scan ;
    echo "- - -" > /sys/class/scsi_host/host1/scan ;
    echo "- - -" > /sys/class/scsi_host/host2/scan ;
    echo "- - -" > /sys/class/scsi_host/host3/scan ;
    echo "- - -" > /sys/class/scsi_host/host4/scan
###### Scan on all controllers for disks
    for a in $(ls /sys/class/scsi_host/); do echo "- - -" > /sys/class/scsi_host/$a/scan ; done

##### Safety remove of Hard Disk (Online)
###### Remove PV from VG
    vgreduce vgtest /dev/<device>
###### Remove PV from LVM
    pvremove /dev/<device>
###### Delete block device
    echo 1 > /sys/block/<device>/device/delete
###### Identify the controller and port
    lsscsi -sg
###### If lsscsi is not available, you can list the devices by path
    ls -l /dev/disk/by-path/

##### Check for diferences between partition table and LVM
    for a in $sitedev ; do ssh root@$a 'fdisk -l | grep vgstl1 | grep Disk | sort ; echo "-----------------------" ; lvs | grep vgstl1 | sort' ; done

## Process Management
- **process**: A running application or command.
- **job**: A process started by the user from the command line.
- **priority**: The right to claim system resources (CPU, memory) one process has relative to other processes. niceness: A way of representing priority, on a scale from -20 to 20, with -20 being the highest priority, and most user started processes having a niceness of 0.
- **foreground**: A process started by typing a command into the terminal is generally running in the foreground by default: you wait until it is finished before executing further commands. A process started in the terminal and running in the foreground can be stopped with the “Ctrl+z” key combination.
- **background**: Background processes do not prevent you from starting additional processes. Commands can be executed in the background by appending an “&” to the end of the command.
- **jobs**: The jobs command gives a numbered list of the user initiated commands that are running in the background
- **fg**: The fg command returns a background process to the foreground. If multiple commands are running in the background, use the number given by the jobs command to specify which job you want foreground.
- **top**: A command for checking process resource utilization. By default, displays processes using the most CPU, use “shift+m” to display by memory usage instead.
- **ps**: The process snapshot command gives a list of running processes.
- **nice**: The nice command is used to change the relative priority of a process when starting the process.
- **renice**: The renice command is used to change the relative priority of a process that is already running.
- **kill**: The kill command stops processes. By default, kill sends a hangup signal to the process. Used with the -9 option, a process is force killed via the kernel directly, rather than by interaction with the process itself.
- **nohup**: The nohup command can be used to start jobs that don’t respond to hangup signals, i.e. commands that need to run after the user starting them has logged out.

##### List the running processes that aren’t owned by root.
    ps -U root -N
#####  Run an update with highest priority
    nice -20 yum update -y
##### Watch disk utilization
    # watch df -h
    Every 2.0s: df -h
    Filesystem Size Used Avail Use% Mounted on
    /dev/mapper/VolGroup-lv_root
     19G 792M 17G 5% /
    tmpfs 246M 0 246M 0% /dev/shm
    /dev/vda1 477M 46M 406M 11% /boot
    Press “ctrl+z” to suspend the watch job.
    Use the jobs command:
    # jobs
    [1]+ Stopped watch df -h
    Use the fg command to foreground the watch job:
    #fg
    Every 2.0s: df -h
    Filesystem Size Used Avail Use% Mounted on
    /dev/mapper/VolGroup-lv_root
     19G 792M 17G 5% /
    tmpfs 246M 0 246M 0% /dev/shm
    /dev/vda1 477M 46M 406M 11% /boot

##### List current jobs
    jobs
##### Use top command and sort by memory
    top
    "shift + m"
