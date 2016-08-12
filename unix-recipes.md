# Unix Recipes

#### System input/output
##### Check FS’s usage
    iostat
##### Sed
    sed 's/<word_to_replace>/<new_word>/g'
##### Process Tree
    proctree <pid>
##### Remount root filesystem with write options (usefull i runlvl0)
    mount -o remount,rw /

#### Memory
##### Check which process is using all RAM**
    ps aux --sort=-%mem
##### Check overall RAM status
    free -m

----
#### CPU
##### Check which process is using all CPU
    ps aux --sort=-%cpu

----
#### Networking
##### Finding all network connections in files
    lsof -i
##### List all network files in use by a specific port
    lsof -i <port>
    lsof -n -i 4TCP:<$PORT>  
##### List all network connections**
    netstat -a
##### Display active connections**
    netstat -arnp | grep ESTABLISHED
    # or watch it continuous
    watch -d -n0 "netstat -arnp | grep ESTABLISHED"
##### Get process id/name and user id associated to active connection
    netstat -nlpt
##### Display kernel routing information
    netstat -rn
##### List all TCP or UDP connections
    netstat -at
    # or
    netstat -au
##### List all listening connections**
    netstat -tnl 
##### Sniff trafic on the specified port and host
    tcpdump -ne port <port> and host <host>

----
#### Files
##### Sync an entire directory
    rsync -a <from_directory> <to_directory>
##### Check all files size for the current directory
    du -hs *
##### List opened files under directory
    lsof +D /path/to/directory
##### List opened files by user
    lsof -u <user>
##### List opened files based on process name
    lsof -c <process name>
##### List opened files based on process id
    lsof -p <pid&gt;
##### Kill all process’s that belong to a particular user
    kill -9 `lsof -t -u <username>`
##### List all network files in use by a specific process
    lsof -i -a -p <pid>
    # or
    lsof -i -a -c <process_name>
##### List all NFS files by user
    lsof -N -u username -a
##### Remount root filesystem with write options
    mount -o remount, rw /
