# ZFS

#### Good information about ZFS:
- [Cheat sheet](http://thegeekdiary.com/solaris-zfs-command-line-reference-cheat-sheet/)_
- [Tutorial 1](https://flux.org.uk/tech/2007/03/zfs_tutorial_1.html)
- [Tutorial 2](https://flux.org.uk/tech/2007/03/zfs_tutorial_2.html)

#### ZFS Basics
##### There are two main commands
- zpool - manages zfs pools and devices within them 
- zfs - manage zfs filesystems

##### List pools
    zpool list
##### Create pool from disk /root/disk1
    zpool create <pool name> /disk/to/path
##### Create a filesystem within our ZFS pool
     zfs create <pool name>/<new lv>

##### Check all available parameters
    zfs get all <pool name>/<lv>
##### Set mount point for ZFS filesystem
    set mountpoint=**/path/to/mountpoint** <pool name>
##### Unmount ZFS filesystem
    zfs unmount <pool>/<lv>
##### List pools status
    zpool status
##### List a specific pool status
    zfs list <pool name>
##### Extend pool size with another disk
    pool add <pool name> /path/to/new/disk
##### Delete pool
    zpool destroy <pool name>

#### ZFS Mirroring

When something is written on first disk it will be written on the second one, this can be useful when you want to do upgrades or just have redundancy on your filesystems. “Mirroring” allows us to use a lot of raid types, check this [link](http://www.zfsbuild.com/2010/05/26/zfs-raid-levels/) for more information.

##### Create a pool with mirroring 
     zpool create <pool name> mirror /path/to/first/disk /path/to/second/disk
##### Checking for errors on disks
    zpool scrub

Now if we list the status of our pool it will display something like this...

##### Detach a disk from a pool
     zpool detach <pool name> /path/to/disk

##### Add a new disk and remirror it 
Now we need to "remirror" the disk, this operation is called "resolver".
     zpool attach <pool name> /disk/to/mirror/from /path/to/new/disk
Now if we check the status, it will show as online again...

##### Add a new disks to an existing pool in mirror state
    zpool <pool name> mirror /path/to/new/disk1 /path/to/new/disk2

#### ZFS Proprieties
##### Get all information
    zfs get all <pool or FS name>

- **default** - the default ZFS value for this property
- **local** - the property is set directly on this filesystem
- **inherited** - the property from a parent filesystem

#### Quotas & Reservations
##### Set a quota
A quota allows us to set a limit on a pool space. A quota **shouldn’t be bigger** than his reservation. 
    set quota=<space> <pool/fs>

##### Get quota values
    get quota <pools> <filesystems>  

##### Set a reservation
A reservation reserves part of the pool for the exclusive use of one filesystem.
    set reservation=<space> <pool/fs>

##### Get reservation values
    get -rH  reservation <pool or filesystem>

- **-r** - recursively, gets the property for all child filesystems
- **-H** - omits headers files, making the output easier for scripts
- **-o** - specify a list of fields you wish to get  
