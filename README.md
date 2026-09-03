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
