# autologsh

Simple shell script/lib (bash/zsh/ksh) to add timestamp to output of command and also save in a log file managing directly logrotate of log file.
Used as lib, source in a shell script to automatically have timestamped stdout/stderr output and also saved in a log file at script execution.

## features

Using `autologsh <command> [<args>]`, or just sourcing `autologsh` in as shell script provides:

* timestamped command/shell script output (displayed on stdout) 
* timestamped command/shell script to log file
* logrotate log file on `maxsize` keeping `nrotate` compressed files (customizable)
* customizable timestamp format (`timeformat`)
* `log` function to format message with level/facility/message
* compatible bash/zsh/ksh

## usage 
as a command:
```
[autologsh variables] autologsh [--nolog] <command> [<args>]
or as piped output logging:
command | autologsh [--nolog] -
```
`--nolog` parameter will just display timestamped output without logging into a log file (like `ts` linux command)

as a lib, in a bash script, just put :
```
[autologsh variables] source <path>/autologsh [--nolog]
```
variables default values:
```
logfile=<pathtologfile>    ~/.autolog/<callingscript>.log
maxsize=<maxsize>          5M
norotate=<nrotate>         4
timeformat=<timeformat>    [%Y-%m-%d %H:%M:%S]
```
the log file will be automatically rotated when exceeding `maxsize`, keeping `nrotate` compressed history files.


## pre-requisites

* logrotate command
* awk command with strftime()/fflush() functions (gawk/mawk)

## example

the `autologsh` script can be used to timestamp+log directly a command:
```
autologsh ping -c 3 172.26.48.140
[2026-09-05 09:12:43] PING 172.26.48.140 (172.26.48.140) 56(84) bytes of data.
[2026-09-05 09:12:43] 64 bytes from 172.26.48.140: icmp_seq=1 ttl=64 time=0.268 ms
[2026-09-05 09:12:44] 64 bytes from 172.26.48.140: icmp_seq=2 ttl=64 time=0.467 ms
[2026-09-05 09:12:45] 64 bytes from 172.26.48.140: icmp_seq=3 ttl=64 time=0.413 ms
[2026-09-05 09:12:45]
[2026-09-05 09:12:45] --- 172.26.48.140 ping statistics ---
[2026-09-05 09:12:45] 3 packets transmitted, 3 received, 0% packet loss, time 2036ms
[2026-09-05 09:12:45] rtt min/avg/max/mdev = 0.268/0.382/0.467/0.084 ms
```
log saved in `~/.autolog/ping.log`, use `logfile=<logfile> autologsh <command>` to change log file destination.  
use `autologsh --nolog <command>` to have just timestamped output without logging into a file. (like `ts` command)


```shell
#!/bin/bash
logfile=~/log/app.log maxsize=5M nrotate=4 source ~/autologsh/autologsh

echo "Message to display with timeestamp and log to logfile"
log INFO myscript "log function to easy format log with level description message"
ping -c 3 172.26.48.143
log ERROR myscript "This is an Error message"
```
executing the script is giving following output:
```
[2026-09-04 00:49:28] Message to display with timestamp and log to logfile
[2026-09-04 00:49:28] INFO  myscript   log function to easy format log with level description message
[2026-09-04 00:49:28] PING 172.26.48.143 (172.26.48.143) 56(84) bytes of data.
[2026-09-04 00:49:28] 64 bytes from 172.26.48.143: icmp_seq=1 ttl=64 time=0.369 ms
[2026-09-04 00:49:29] 64 bytes from 172.26.48.143: icmp_seq=2 ttl=64 time=0.528 ms
[2026-09-04 00:49:30] 64 bytes from 172.26.48.143: icmp_seq=3 ttl=64 time=0.506 ms
[2026-09-04 00:49:30]
[2026-09-04 00:49:30] --- 172.26.48.143 ping statistics ---
[2026-09-04 00:49:30] 3 packets transmitted, 3 received, 0% packet loss, time 2049ms
[2026-09-04 00:49:30] rtt min/avg/max/mdev = 0.369/0.467/0.528/0.070 ms
[2026-09-04 00:49:30] ERROR myscript   This is an Error message
```
each execution also logged in `~/log/app.log`
```
---------------------------------
[2026-09-04 00:48:27] Message to display with timestamp and log to logfile
[2026-09-04 00:48:27] INFO  myscript   log function to easy format log with level description message
[2026-09-04 00:48:27] PING 172.26.48.143 (172.26.48.143) 56(84) bytes of data.
[2026-09-04 00:48:27] 64 bytes from 172.26.48.143: icmp_seq=1 ttl=64 time=0.328 ms
[2026-09-04 00:48:28] 64 bytes from 172.26.48.143: icmp_seq=2 ttl=64 time=0.501 ms
[2026-09-04 00:48:29] 64 bytes from 172.26.48.143: icmp_seq=3 ttl=64 time=0.714 ms
[2026-09-04 00:48:29]
[2026-09-04 00:48:29] --- 172.26.48.143 ping statistics ---
[2026-09-04 00:48:29] 3 packets transmitted, 3 received, 0% packet loss, time 2051ms
[2026-09-04 00:48:29] rtt min/avg/max/mdev = 0.328/0.514/0.714/0.157 ms
[2026-09-04 00:48:29] ERROR myscript   This is an Error message
---------------------------------
[2026-09-04 00:49:28] Message to display with timestamp and log to logfile
[2026-09-04 00:49:28] INFO  myscript   log function to easy format log with level description message
[2026-09-04 00:49:28] PING 172.26.48.143 (172.26.48.143) 56(84) bytes of data.
[2026-09-04 00:49:28] 64 bytes from 172.26.48.143: icmp_seq=1 ttl=64 time=0.369 ms
[2026-09-04 00:49:29] 64 bytes from 172.26.48.143: icmp_seq=2 ttl=64 time=0.528 ms
[2026-09-04 00:49:30] 64 bytes from 172.26.48.143: icmp_seq=3 ttl=64 time=0.506 ms
[2026-09-04 00:49:30]
[2026-09-04 00:49:30] --- 172.26.48.143 ping statistics ---
[2026-09-04 00:49:30] 3 packets transmitted, 3 received, 0% packet loss, time 2049ms
[2026-09-04 00:49:30] rtt min/avg/max/mdev = 0.369/0.467/0.528/0.070 ms
[2026-09-04 00:49:30] ERROR myscript   This is an Error message
```
