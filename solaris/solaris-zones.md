# Solaris Zones

### Main Commands
#### Configure & Setup Zones
    zonecfg 
#### List all available zones
    zoneadm list -cv  
#### Power On/Off, install, uninstall, attach and detach
    zoneadm -z <zone name> boot | halt | install | uninstall | attach | detach**  
#### Login into zone console
    zlogin -C <zone name> 
#### Login into zone shell
    zlogin -z <zone name>

### Zone Clonning
#### Power off the zone that we want clone
    zoneadm -z <zone name> halt

#### Export his configurations to config file
    zonecfg -z <zone name> export -f ~/<file name>

#### Edit the configuration file, and set the FS mount point and IP address
    vi ~/<file name>

#### Create a new zone based on the exported config file
    zonecfg -z pum -f ~/<file name>

#### Clone the installation from our first zone to our new zone 
    zoneadm -z <new zone name> clone <zone name>

#### Boot up our zones
    zoneadm -z pim boot
