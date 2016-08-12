# NFS Server Basics

#### Main configuration file: /etc/exports
This will save our exports, the structure should look like this:
    <directory>          <host1(options)> <host2(options)>

#### Common Export Options
- ro/rw: Read Only/Read and Write
- async/sync: Allow server to cache writes/Force write to disk
- root_squash Requests from root clients are mapped to the _nobody_ user and group
- no_subtree_check: If only part of a volume is exported, a routine called _subtree checking_ verifies that a file that is requested from the client is in the appropriate part of volume. If the entire volume is export, disabling this will speed up transfers.

Reload exports without restart the NFS service
    exportfs -r

--
###### Note:
- Design fiilesystems with **growth in mind**
- Distribute filesystems across multiple servers
- $$ spent on hardware will payoff
