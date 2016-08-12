# Extend Swap over LVM
## Extend Swap over LVM
**Note: This operation can't be done with Swap online**
##### Lists available Swap's &amp; turn off the Swap that you want extend
    spaw -s
    swapoff /dev/dem-1

##### Display all LV's (Logical Volumes)
    lvdisplay

##### Extend Swap's LV (Logical Volume)
    lvextend -L 4G /dev/rootcg/lfswap

##### Make a FS of Swap type &amp; turn it on again
    mkswap /dev/rootcg/lfswap
    swapon /dev/rootcg/lfswap
