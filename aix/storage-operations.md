# AIX Storage Operations

####  Assign Disk and Space to LV
#####  List all physical volumes
    lspv
#####  Discover new disks
    cfgmgr [-e show output]
#####  Display volume paths
    powermt display path
#####  List disk attributes
    lsattr -El <disk>
##### Extend VG (Volume Group)
    cl_extendvg
##### Check space available on disk and is VG name
    df -gt /path/to/lv
    lslv <lv name>
##### Check VG space
    lsvg <vg name>
##### Get resource
    cl_lsvg | grep -w <vg name>
##### Assign space to LV
    cl_chfs -cspoc "-g <resource group name> -a size=<+/- disk spaceM/G> /path/to/logical/volume

--
#### PV Migration
##### Macro Operations

- Scan for new disks
- Setup new disks (algoritmh - RR, reserved policy - no reserve)
- Extend VG with new PV
- Migrate old PV into new PV
- Remove old PV from VG

###### Note:
- For non cluster operations, just remove the -cspoc flag and the node name.
- All commands started by **cl_** can be executed in any node, the rest must be execute on the node which the VG is online.

##### Discover new disks
    cfgmgr [-e show output]
##### Extend VG (Volume Group)
    cl_extendvg -cspoc "-g <resource group>" <-R node> <volume group> [disks...]
##### Migrate Physical Volume
    migratepv <old pv> <new pv>
##### Reduce VG (Volume Group)
    cl_reducevg -cspoc "-g <resource group>" -R <node> <volume group> [old disks...]
##### Remove PV ID from old disks
    chdev -a pv=clear -l <old disk>
##### Remove PV ID from old disks, this operation must be done on _all cluster nodes_
    rmdev -dl <old disk>

--
#### Aumentar Storage (non cluster)
##### Ver se os Max PPs e o limite máximo de PV’s do VG
    lsvg <vgname>
##### Discovery dos novos devices nas HBA's
    cfgmgr
##### Enquire à VMAX pelos novos discos
    /path/to/EMc/enquire/software/inq.aix64_51 -no_dots -sym_wwn >** disks.jdc
##### Adicionar atributos aos PV’s (Algorithm, PV, Reserve Policy)
    chdev -a pv=yes -a algorithm=round_robin -a reserve_policy=no_reserve -l **<device>**
##### Checkar o tamanho da Lun
    bootinfo -s <disco>
##### Extend ao VG
    extending <vg name> <disks>
##### Aumentar o LV
    chfs -a size=+250G /mount/point/lv

_Caso o LV tenha batido no máximo de LP’s, é necessário alterar o **Max LPs**_

##### Mudar o máximo de LP’s (logical partitions)
	chlv -x <numero de lp’s> <lv name>
##### Reorganizar a data pelos PV’s do VG
	reorgvg <vg name>

--
#### Migrate rootvg
##### Extend VG with a new PV
    extendvg **<vg name>** **<new pv>**
##### List all logica devices
     lsvg -l rootvg
##### Migrate boot to new PV
    migratepv -l hd5 **<old pv>** **<new pv>**
##### Clean all boot information from old pv
    chpv -c **<old pv>**
##### Check if the new disk has boot device
    bootlist -m normal -o **<new pv>**
##### Install boot on new PV
    bosboot -ad /dev/**<new pv>**
##### Migrate all data from old pv to new pv
    migratepv **<old pv>** **<new pv>**
##### Reduce VG
    reducevg rootvg **<old pv>**
##### Might be needed to run [savebase](https://www.ualberta.ca/dept/chemeng/AIX-43/share/man/info/C/a_doc_lib/cmds/aixcmds5/savebase.htm), to save information in Device Configuration database onto the boot device.

--
#### Delete Volume Group
##### _Important_: Save all PV’s to the VG on both nodes.
##### Varyoff the Volume Group
    varyoff **<volume group>**
##### Delete Volume Group from ODM and any other reference (/etc/filesystems, etc…)
    exportvg **<volume group>**
##### Remove PV ID from the PV’s attached to the Volume Group
    chdev -a pv=clear -l **<disk>**
##### Write zero blocks on the old devices
    dd if=/dev/zero of=/dev/**<disk>** 1024
##### Remove PV ID from the other node
    chdev -a pv=clear -l **<disk>**

--
#### Create Volume Group, Logical Volume and Filesystem
##### Create Volume Group
    mkvg -c -s **<PP Size>** -V 97 -S -y **<VG Name>** [Disks]
##### Learn new VG on the other node
    importvg -V 97 -y <VG Name> **<any available disk on vg>**
##### Add new VG to Resource Group over Smitty
    smitty hacmp
##### Create Logical Volume
    cl_mklv -cspoc "-g <Resourse Group>" -y <LV Name> -e x -t jfs2 <VG Name> 209472M
##### Create Filesystem
    cl_crfs -cspoc "-g <Resourse Group>" -v jfs2 -d <LV Name> -m "/mount/point" -A no -p rw -a logname=INLINE

--
#### Notes:
**ODB** - AIX devices DB
**cfgmgr** - Devices discovery and insert pride’s into ODB
**cl_extendvg/cl_chfs** - When used in cluster, the reference node must be specified and cscops
**cspoc** - All cluster operations must use this flag, for non cluster, just remove cspoc flag and the node

--
#### Macros:
##### list all hdiskpower from VG in line
    lsvg -p <vgname> | grep hdiskpower | awk '{print $1}' | xargs
##### list all hdisk from VG in line
    lsvg -p <vgname> | grep -v hdiskpower | grep hdisk | awk '{print $1}' | xargs
##### inquire EMC
    /path/to/storage/enquire/software/inq.aix64_51 -no_dots -sym_wwn >** disks.jdc
##### prepare migratepv’s in bulk
    vg2mig="**<vg name>**"; for a in $(lsvg -p $vg2mig | grep hdiskpower | awk '{print $1}' | xargs) ; do echo migratepv $a $(lsvg -p $vg2mig | grep -v hdiskpower | grep hdisk | awk '{print $1}' | xargs); done
##### migrate lp’s in bulk
    dsk2free=**<DISK>** ; dsk2fill=**<DISK>** ; for a in $(lspv -M $dsk2free | head -4 | awk '{print $2}' | sed 's/:/\//g') ; do migratelp $a $dsk2fill ; done
##### list all local vg's
    lsvg | egrep -v "$(cl_lsvg | grep -v Resource | awk '{print $2}' | xargs | sed 's/ /|/g')"
