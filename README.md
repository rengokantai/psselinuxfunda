#### psselinuxfunda
#####selinux bp
######selinux modes
disabled  
permissive(not enforcing the policy, logs what would have been denied)  
enforcing(enforcing the policy)
######modes of cli
```
getenforce
setenforce(1,0)  1=enforcing 0 =permissive
sestatus
```
permanent change
```
vim /etc/selinux/config
```
restart.
```
shutdown -r now
```

######selinux labels
user:role:type:level  

show se labels
```
id -Z
ps -Z
ls /etc/passwd -Z
```
######type
targeted minimum mls  
```
SELINUXTYPE=targeted
######users
```
seinfo -t
seinfo -u
seinfo -r
```
```
semanage login -l
```

