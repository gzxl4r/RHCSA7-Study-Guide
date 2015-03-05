# RHCSA7-Study-Guide
## Things I need to go over


###Attaching a system to centralized LDAP and Kerberos servers

1 . Install packages

```bash
yum -y install authconfig-gtk sssd krb5-workstation
```

2. Launch authconfig-gtk

3. Fill in the information, and remember that the base DN needs to be in this format: 

```shell
dc=example,dc=com
```

###Special Permissions

1. u+s (suid) - File executes as the user that owns the file, not the user that ran the file. 

2. g+s (sgid) - Files newly created in the directory have their group owner set to match the group owner of the directory.

3. o+t (sticky) - Users with write on the directory can only remove files that they own; they cannot remove or force saves to files owned by other users. 

###POSIX Access Control Lists

1. File system mount option - the 'acl' mount option is included by default on RHEL7 for ext4 file systems

2. ls -l outputs ACL setting details. The "+" at the end of the permission string indicates that there are ACL settings on the file

3. getfacl <file> displays all ACL setings

4. Commands: 

```bash
setfacl -m g:name:rw <file>
```

```bash
setfacl -m o::- <file>
```

```bash
setfacl -m u::rwx,g:sodor:rX,o::- <file>
```

```bash
getfacl <file-a> | setfacl --set-file=- <file-b>
```

```bash
setfacl -m m::r file
```

```bash
setfacl -R -m u:name:rX <directory>
```
Delete an ACL
```bash
setfacle -x u:<username>,g:<groupname> <file>
```
Delete all ACL
```bash
setfacl -b <file>
```

###SELinux

####SELinux Modes

1. Enforcing - SELinux both logs and protects 

2. Permissive - Allows all actions, logs interactions it would have denied in enforcing mode

3. disabled - completely disables SELinux

4. Commands

```bash
getenforce
```

```bash
setsebool -a
```

```bash
setenforce <Enforcing|Permissive| 1 | 0 >
```

#####Changing the default SELinux mode

* /etc/selinux/config

#####Changing the SELinux Contexts

* man -k '_selinux' to checkout selinux booleans

```bash
chcon -t <type_t> <file>
```

```bash
restorecon -v <directory>
```

Change the default context rule of a direcotry and everything in the directory

```bash
semanage fcontext -a -t <context_type> '<dirctory>(/.*)?'
```

#####Troubleshooting SeLinux
THe most common SELInux issues is an incorrect file context. This can occur when a file is created in a location with one file context and moved into a place where a different context is expected. In most cases, running restorecon will correct the issue.

Product reports for all incidents in that file. 
```bash
yum install setroubleshoot-server -y
sealert -a /var/log/audit/audit.log
```



