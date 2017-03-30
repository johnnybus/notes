Pre-BPF: Linux Perf Analysis in 60s
===================================
This list is a slide from a Bredan Gregg presentation.
To have more context take a look at [this](http://techblog.netflix.com/2015/11/linux-performance-analysis-in-60s.html) and [this](http://www.brendangregg.com/blog/2016-03-05/linux-bpf-superpowers.html) presentation .

1. uptime
2. dmesg -T | tail
3. vmstat 1
4. mpstat -P ALL 1
5. pidstat 1
6. iostat -xz 1
7. free -m
8. sar -n DEV 1
9. sar -n TCP,ETCP 1
10. top
