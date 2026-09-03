# autologsh

Simple bash lib to source in a script to automatically have stdout/stderr in a log file with timestamp and having it displayed at script execution.

Manage logrotate of log file

## usage 
in a bash script, just put :
```
[autologsh variables] source <path>/autologsh
possible variables (can also be declared before sourcing) :
[logfile=<pathtologfile>] [maxsize=<maxsize>] [nrotate=<nrotate>] [timeformat=<timeformat>] source <path>/autologsh
```
default values:
```
<pathtologfile> ~/.autolog/<callingscript>.log
<maxsize>       5M
<nrotate>       4
<timeformat>    [%Y-%m-%d %H:%M:%S]
```
the log file will be automatically rotated when exceeding `maxsize`, keeping `nrotate` compressed history files.

## pre-requisites

* logrotate command
* awk command with strftime() function (gawk/mawk)

## example

```shell
#!/bin/bash
logfile=~/log/app.log maxsize=5M nrotate=4 source ~/autologsh/autologsh

echo "Message to display with timeestamp and log to logfile"
log INFO myscript "log function to easy format log with level description message"
log ERROR myscript "This is an Error message"
```
executing the script is giving following output:
```
[2026-09-03 23:41:12] Message to display with timestamp and log to logfile
[2026-09-03 23:41:12] INFO  myscript   log function to easy format log with level description message
[2026-09-03 23:41:12] ERROR myscript   This is an Error message
```
each execution also logged in `~/log/app.log`
```
---------------------------------
[2026-09-03 23:41:12] Message to display with timestamp and log to logfile
[2026-09-03 23:41:12] INFO  myscript   log function to easy format log with level description message
[2026-09-03 23:41:12] ERROR myscript   This is an Error message
---------------------------------
[2026-09-03 23:43:19] Message to display with timestamp and log to logfile
[2026-09-03 23:43:19] INFO  myscript   log function to easy format log with level description message
[2026-09-03 23:43:19] ERROR myscript   This is an Error message
```

