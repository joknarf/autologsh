# autologsh

Simple bash lib to source in a script to automatically have stdout/stdin in a log file with timestamp and having it displayed at script execution.

Manage logrotate of log file

## usage 
in a bash script, just put :
```
logfile=<pathtologfile> [maxsize=<maxsize>] [nrotate=<nrotate>] source <path>/autologsh
```
default values:
```
<pathtologfile> ~/.autolog/log_<tty>
<maxsize> 5M
<nrotate> 4
```
the log file will be automatically rotated when exceeding `maxsize`, keeping `nrotate` compressed history files.

## pre-requisites

* logrotate command
* awk command with strftime() function (gawk/mawk)

## example

```shell
#!/bin/bash
logfile=~/log/app.log maxsize=5M nrotate=4 source ~/autologsh/autologsh

echo "Message to log with timestamp
log INFO myscript "log function to easy format log with level description message"
log ERROR myscript "This is an Error message"
```
executing the script is giving following output:
```
[2026-09-03 23:41:12] Message to log with timestamp
[2026-09-03 23:41:12] INFO  myscript   log function to easy format log with level description message
[2026-09-03 23:41:12] ERROR myscript   This is an Error message
```
each execution also logged in `~/log/app.log`
```
---------------------------------
[2026-09-03 23:41:12] Message to log with timestamp
[2026-09-03 23:41:12] INFO  myscript   log function to easy format log with level description message
[2026-09-03 23:41:12] ERROR myscript   This is an Error message
---------------------------------
[2026-09-03 23:43:19] Message to log with timestamp
[2026-09-03 23:43:19] INFO  myscript   log function to easy format log with level description message
[2026-09-03 23:43:19] ERROR myscript   This is an Error message
```

