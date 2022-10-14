---
title: "Tuning System Performance in Linux"
date: 2022-10-14T02:35:59+03:00
draft: true
weight: 10
summary: >
    Red hat Enterprise Linux tutorial notes
    <<KILL ☼ TUNING ☼ SCHEDULING>>
author: 'Evans Owamoyo'
categories: [Notes]
tags: [linux, rhel]
---
## Signal
A signal is a software input delivered to a process
| id | code | description |
|--|----|---------------|
|1 |SIGHUP| Hangup - report term of the process of terminal.|
|2 |SIGINT |keyboard interrupt ctrl + C|
|3 |SIGQUIT |quit ctrl + \|
|9 |KILL| kill unblockable|
|15| SIGTERM |terminate (allows self cleanup)|
|18 |SIGCONT |continue|
|19| SIGSTOP| stop, unblockable|
|20| TSTP |keyboard stop|

* `pkill` is used to send a signal to one or more processes which match selection criteria. (command name, a process owned by a user, all system-wide processes)
* `pgrep` like pkill list processes but does not kill them.

* `w` lists user's login sessions
 - in TTY section **pts/N** represent graphical term or remote login session while **ttyN** represents system console / alternate console / term device

* `uptime` is used to display the current load average. It prints the current time, how long the machine has been up, how many user sessions are running, and the current load average. 1,5,15 mins 

* The `top` program is a dynamic view of the system's processes


## PERFORMANCE TUNING
Tuned provide profiles: Power-saving and Performance boosting
Tuned profiles:
- balanced (compromise between power saving and performance)
- desktop (faster response of interactive apps)
- througput-performance (max throughput)
- latency-performance (ideal for servers that require low latency but consumes power)
- network-latency (from latency-perf, but requires add network tuning)
- network-throughput (from throughput, add. network tuning for max network throughput)
- powersave (max powersaving)
- oracle (optimized for oracle database loads based on throughput-performance)
- virtual-guest (tunes for max perf. if it runs on vm)
- virtual-host (tunes for max perf. if it acts as a host for vms)

* tuned-adm is used to change tuned daemon settings
tuned-adm list , tuned-adm profile <profile>, tuned-adm active , tuned-adm recommend

## INFLUENCING PROCESS SCHEDULING 
default scheduling policy: SCHED_OTHER (SCHED_NORMAL), processes can still be given a relative priority. (nice value -20<=x<20) usually 0

* Higher nice value means lower priority, (easily gives up CPU usage - nice guy)
* only a root user can reduce the nice level of a process. Unprivileged users can increase nice level of their own processes.

## Displaying nice level
```bash
ps -o pid,comm,nice,pcpu
```


