# Unix Monitoring 
## VMSTAT

##### Command: vmstat <interval in seconds>
##### Description: Report virtual memory statistics and information about system events such as CPU load, paging, number of context switches, system calls, swapping, cache flushing.
##### "kthr"
- r - number of kernel threads in the dispatch queue 
- b  - number of blocked kernel threads that are waiting for resources
- w - number of swapped out LWP’s that are waiting for processing resources to finish

##### "memory"
- free (physical ram) - size of free ram in KB
- swap (swap) - available swap in KB

##### "cpu"
- us (user) - cpu percentage used by user
- sy (system) - cpu percentage used by system
- id (idle) - cpu percentage in idle
- "page"
- re - pages reclaimed 
- mf - minor and major faults 
- pi - Kbytes paged in
- po - Kbytes paged out
- fr - Kbytes feed
- de - anticipated memory that is needed by recently swapped-in processes
- sr (scan rate) - pages scanned by the page daemon not currently in use, if **sr** equal zero the daemon has been running, scan rate is the way that the system uses to recover memory when there’s memory pressure




##### "page"
s0/s1/sX** - reports number of disk operations per second, showing data on up to four disks
page - page, memory page or virtual page is block of virtual memory, described by a single entry in the page table, it is the smallest unit of data for memory management in a virtual memory OS
Command: vmstat -s
Description: Report how many system events have taken place since the last time the system was booted.

Command: vmstat -S
Description: Report of swapping statistics.
- si - average number of LWP’s that are swapped in per second
- so - average number of LWP’s that are swapped out per second

Command: vmstat -i
Description: Show the number of interrupts per device


## PS
Command: ps
Description: Displays information about a selection of **active processes**.

##### Sort by consumed RAM
    ps -aux —sort=-%mem

##### Sort by consumed CPU
    ps -aux —sort=-%cpu

##### UPTIME & LOAD AVERAGES
Command: uptime
Description: Show the uptime, current logged in users and the **load average**.

###### Sample output
    load average: <last minute>, <last 5 minutes>, <last 15 minutes>

- over the last 1 minute: the computer was overloaded by 1% on average, so .01 processes were waiting for the CPU
- over the last 5 minutes: the computer was overloaded by 1% on average, so .01 processes were waiting for the CPU
- over the last 15 minutes: the computer was overloaded by 1% on average, so .01 processes were waiting for the CPU

##### How this  works?!
For multiple CPUs or multi-core CPU the load average numbers work a bit differently on such a system. For example, if you have a load average of 2 on a single-CPU system, this means your system was overloaded by 100% - the entire period of time, one process was using your CPU while one other process was waiting.

##### load average / number of CPUs  
On a system with two CPUs, this would be complete usage - two diferente processes were using two different CPUs the entire time. On system with four CPUs, this would be half usage - two processes were using two CPUs, while two CPUs were setting idle. If the load average per core is over 1, it means that your system is overloaded.
