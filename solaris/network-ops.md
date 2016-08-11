# Solaris 11 - Network Operations

**dladm** - perform data-link (layer 2) administration to configure physical links, aggregations, VLAN’s, IP Tunnels, InfiniBands Partitions, also manages link-layer properties.

**ipadm** - configures IP interfaces, IP address and TCP/IP protocol properties.

### Network Setup Solaris 11
#####  List network profiles current in use
    netadm list
##### Set network to DefaultFixed
    netadm enable -p ncp DefaultFixed
##### Show physical NIC's
    dladm show-physis
##### Show IP configuration
    ipadm
#####  Create an IP interface
    ipadm create-ip net0
##### Create an static IP address on NIC
    ipadm create-addr -T static -a 10.0.2.15/24 net0
##### Create an DHCP IP address
    ipadm create-addr -T dhcp net0
##### Create an aggregate
https://docs.oracle.com/cd/E26502_01/html/E28993/gkaap.html#gafxi
-L flag will set the state of LACP status
if the router/switch supports LACP, set it active
    dladm create-aggr -L off -l net1 -l net2 aggr0
##### Create an IP interface on the aggregate
    ipadm create-ip aggr0
##### Set an IP address on aggregate interface
    ipadm create-addr -T static -a 192.168.99.130/24 aggr0


### Config DNS Client Options over svccfg
##### List DNS client, property search
    svccfg -s svc:/network/dns/client listprop config/search
##### Set DNS Client, property search equals to...
    svccfg -s svc:/network/dns/client setprop config/search='("groupinfra.com" "ptinfra.com" "appl.edinfor.pt")'
##### Set DNS Client, nameservers property equals to...
    svccfg -s svc:/network/dns/client setprop config/nameserver=net_address: '(10.167.162.20 10.167.162.36)'
##### List DNS Client, nameservers property
    svccfg -s svc:/network/dns/client listprop config/search
##### Restart DNS Client
    svcadm refresh svc:/network/dns/client

### Services Management
##### Lists all services
    svcs -a
##### Lists dependecies of an FRMI
    svcs -d &lt;FRMI&gt;
##### Lists dependents of an FRMI
    svcs -D &lt;FRMI&gt;
##### Long list information about an FRMI
    svcs -l &lt;FRMI&gt;
##### Explains why a service is not available
    svcs -x &lt;FRMI&gt;

##### Services operations
clear - Clear faults for FMRI.
disable - Disable FMRI.
enable - Enable FMRI.
refresh - Force FMRI to read config file.
restart - Restart FMRI.
svcadm {enable|disable|restart|refresh|mark|clear|milestone|delegate} &lt;FRMI&gt;

--
Glossary:
FMRI - Fault Managed Resource Identifier
