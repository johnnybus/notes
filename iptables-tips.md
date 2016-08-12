# IPTables Tips
##### List all Rules
    iptables -L
##### {DROP|REJECT|ACCEPT|FORWARD} all connections from a range of IP addresses
    iptables -A <chain> -s 10.10.10.0/24 -j {DROP|REJECT|ACCEPT|FORWARD}
##### {DROP|REJECT|ACCEPT|FORWARD} an connection to a specific port
    iptables -A <chain> -p tcp —dport <port> -s <host> -j {DROP|REJECT|ACCEPT|FORWARD}
###### If you want to apply this rule to all connections, just remove the flag -s


#### Examples
##### INPUT
    iptables -A INPUT -p tcp --dport 22 -s <host> -j ACCEPT

##### NAT
###### PREROUTING
    iptables -tnat -A PREROUTING -p tcp --dport 80 -s 172.17.8.1 -j DNAT --to 22
###### Redirect SSH to another server
    sysctl net.ipv4.ip_forward=1  
    iptables -t nat -I PREROUTING 1 -p tcp --dport 22 -j DNAT --to 172.17.8.10:22
    iptables -t nat -A POSTROUTING -j MASQUERADE


##### Glossary
- OUTPUT - Data from computer to network
- INPUT - Data from network to computer
- FOWARD - Forwards the data from a point to another, this usually is used on routers
- ACCEPT - Allow the connection.
- DROP - Drop the connection, act like it never happened. This is best if you don’t want the source to realize your system exists.
- REJECT - Don’t allow the connection, but send back an error. This is best if you don’t want a particular source to connect to your system, but you want them to know that your firewall blocked them.
- Chain - Group of rules, can be also can be named as table.
- Rules - Manage the kind and which connections are available on a chain.
