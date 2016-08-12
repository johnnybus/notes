# NMAP
##### Scanning multiple targets
    nmap 192.168.0.2, 192.168.0.16, 192.168.0.53

##### Scan in range
    nmap 192.168.0.1-30

##### Scan entire subnet
    nmap 192.168.0.*

##### Scan a list of IP addresses
    nmap -iL host.txt 

_File Format (host.txt)_
    192.168.0.1
    192.168.0.4
    192.168.0.98

##### Scan aggressively 
###### Detects OS, Traceroute, additional services information...
    nmap -A 192.168.0.2

##### OS Detection
    nmap -O thenewboston.com

##### Additional service information
    nmap -sV thenewboston.com

##### Fast port scan
    nmap -F thenewboston.com

##### Scan in range or an interval
    nmap -p 20-25,80,443 thenewboston.com

##### Scan port by name
    scan -phttp,mysql thenewboston.com

##### Scan every single port
    nmap -p- thenewboston.com

##### Only display open ports 
    nmap --open thenewboston.com

##### Saving scan results
    nmap -F -oN Desktop/results.txt  thenewboston.com 

- -oN <Regular text file>
- -oX <XML file>

##### Show verbose
    nmap -v thenewboston.com

##### Other examples:
    nmap -Pn 10.100.146.75
    nmap -PnO 10.100.146.75
    nmap -sS -O -P0 -vv 10.100.146.75
    nmap -sS -O -Pn -vv 10.100.146.75
    nmap -sS -O -Pn -vv 10.100.146.75
