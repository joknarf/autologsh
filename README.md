# autologsh

Simple shell lib to source in a shell script (bash/zsh/ksh) to automatically have timestamped stdout/stderr output and also saved in a log file at script execution.  
Manage logrotate of log file.

## features

Just sourcing `autologsh` in as shell script provides:

* timestamped shell script stdout/stderr output (displayed on stdout) 
* timestamped shell script stdout/stderr to log file
* logrotate log file on `maxsize` keeping `nrotate` compressed files (customizable)
* customizable timestamp format (`timeformat`)
* `log` function to format message with level/facility/message
* compatible bash/zsh/ksh

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

the `autolog` script can be used to timestamp+log directly a command:
```
ping -c 3 172.26.48.140 | ~autolog/autolog
[2026-09-05 09:12:43] PING 172.26.48.140 (172.26.48.140) 56(84) bytes of data.
[2026-09-05 09:12:43] 64 bytes from 172.26.48.140: icmp_seq=1 ttl=64 time=0.268 ms
[2026-09-05 09:12:44] 64 bytes from 172.26.48.140: icmp_seq=2 ttl=64 time=0.467 ms
[2026-09-05 09:12:45] 64 bytes from 172.26.48.140: icmp_seq=3 ttl=64 time=0.413 ms
[2026-09-05 09:12:45]
[2026-09-05 09:12:45] --- 172.26.48.140 ping statistics ---
[2026-09-05 09:12:45] 3 packets transmitted, 3 received, 0% packet loss, time 2036ms
[2026-09-05 09:12:45] rtt min/avg/max/mdev = 0.268/0.382/0.467/0.084 ms
```
log saved in `~/.autolog/autolog.log`, use `cmd |logfile=<logfile> autolog` to change log file destination.

## pre-requisites

* logrotate command
* awk command with strftime() function (gawk/mawk)

## example

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
