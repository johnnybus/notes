# Crontab
##### Create crontab file, and open it with text editor
    crontab -e
##### List crontab entries
    crontab -l
##### Crontab Syntax

*

|

*

|

*

|

*

|

*

|

command  

---|---|---|---|---|---  

min (0-59)

|

hour (0-23)

|

day of month (1-31)

|

month (1-12)

|

day of week (0-6)

(Sunday=0)

|

Command to be executed  




##### Crontab Examples

##### Runs every minute
    *  *  *  *  *  <command>

##### Runs at 30 minutes past the hour
    30  *  *  *  *  <command>

##### Runs at 6:45 am every day
    45 6 *  *  *  <command>

##### Runs at 6:45 pm every day
    45 18 *  *  *  <command>

##### Runs at 1 am every sunday
    00  1  *  *  0  <command>

##### Runs at 1 am every sunday
    00  1  *  *  Sun  <command>

##### Runs at 8:30 am on the first day of every month
    30  8  1  *  *  <command>

##### Runs every other hour on 2nd of July
    00  0-23/2  02  07  *  <command>

##### Special strings that can be used

##### Runs at boot
    @reboot <command>

##### Runs once a year
    @yearly <command>    **OR**     @annually <command>

##### Runs once a month
    @monthly <command>

##### Runs once a week
    @weekly <command>

##### Runs once a day
    @daily <command>   OR   @midnight <command>

##### Runs once an hour
    @hourly <command>

##### Multiple Commands
    @daily <command_1> <command_2>

##### Specify a crontab file to use
    crontab -u <user> <file>

##### Example...
    crontab -u tux ~/crontab

##### Removing a crontab file 
    crontab -r <file>

##### Edit crontab file 
    crontab -e
