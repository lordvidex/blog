---
title: SELinux
date: 2022-10-20T14:48:22+03:00
summary: Security in linux 
author: 'Evans Owamoyo'
categories: [Notes]
tags: [rhel, linux, security]
comments: false
---
## Modes
3 SELinux modes: 
- Enforcing 
- Permissive
- Disabled

## Contexts
Contexts of SELinux: user, role, type and sensitivity

web server: httpd_t, /var/www/html: httpd_sys_content_t,  
{/tmp, /var/tmp}: tmp_t ports : http_port_t  

## Common commands
- `setenforce [1(Enforcing),0(Permissive)]`
- `getenforce` 

setting at boot time, by setting kernel args of `enforcing=0`. You can disable selinux by setting `selinux=0`

the /etc/selinux/config file contains boot configurations for selinux  
```
SELINUX=enforcing|permissive|disabled
SELINUXTYPE=targeted|minimum|mls
```

the moved file maintains its original label while the copied file inherits the label from the `/var/www/html` directory. 

## More commands
Commands to change the SELinux context of a file:
`semanage` `fcontext` `restorecon` `chcon`

* `chcon` - changes the security context on the file, stored in the file system. it does not save when restorecon is run, changes made by chcon is lost.
```bash
chcon -t httpd_sys_content_t /virtual
```
* semanage fcontext can -a --add, -d --delete, -l --list SELinux file contexts
e.g.

```bash
semanage fcontext -a -t httpd_sys_content_t '/virtual(/.*)?'
```

* restorecon sets the SELinux file contexts as defined in semanage
```bash
restorecon -Rv /custom
```

SELinux booleans
*  `getsebool -a [name]` - to get the selinux boolean 
*  `setsebool <name> <on|off>` - to change the value of the boolean (sudo)
*  `semanage boolean -l` - does the same as getsebool (sudo)
* `semanage boolean -l -C (to check diff between current and default)` (sudo)

## Solving and Investigating Issues
`/var/log/messages` and `/var/log/audit/audit.log` receive se linux error messages

```bash
sealert -a /var/log/audit/audit.log 
```
- sealert shows the target file, target context and possible fix

> ausearch  is  a  tool that can query the audit daemon logs based for events based on  different  search  criteria.
e.g. `ausearch -m AVC -ts recent`
