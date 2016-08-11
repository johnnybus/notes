# SVM Storage Operations

## Storage migration over SVM
### Macro Operation
  1. Extend metaset with new devices
  2. Create an new metadevice (will be used as sub-mirror)
  3. Attach the new metadevice to the existing mirror (now, it'll become a sub-mirror)
  4. Wait for resync, delete the old sub-mirror
  5. Delete old metadevice
  6. Remove the LUN from metaset
  7. Remove disks from the host (storage team)
  8. Remove all references to the old device from the device tree

##### List metas on metaset
    metastat -s <metaset> -p
##### Append new devices into metaset
    metaset -s <metaset> -a [device id]

##### Create a new stripe with four disks
###### The number of blocks is used to set a maximum of continuous writes on a disk (round robin)
    metainit -s <metaset> <metadevice name> 1 4 [disks id's] -i <number of blocks>

##### Attach do metadevice ao mirror
    metattach -s <metaset> <mirror> <sub-mirror>

#### Wait until resync finishes
##### Remove old sub-mirror
    metadettach -s <metaset> <mirror> <sub-mirror>

##### Delete old metadevice from metaset
    metaclear -s <metaset> <sub-mirror>

##### Delete old devices from metaset
    metaset -s <metaset> -d <disco>

##### Clean any references to the old disks from device tree
    devfsadm -Cv

##### Check if the disks were actually removed by storage team
    `cfgadm`
