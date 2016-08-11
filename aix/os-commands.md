# AIX OS Commands
##### Error Report
	errpt
###### Important Flags_
- -a - lists detailed info about all errors
- -j - specify an ID

##### Check serial number
    lsattr -El sys0
##### List cluster properties
    cldump
##### Check OS Version
    oslevel -s

### Unlock user authentication
#### List user properties
    lsuser <username>
#### Change user unsuccessful login atempts
    chuser unsuccessful_login_count=0 <username>
#### Change user password
    passwd <username>
