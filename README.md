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
```
######users
```
seinfo -t
seinfo -u
seinfo -r
```
```
semanage login -l
```

#####selinx policies
######target policy
-a: attribute
```
seinfo -aunonfined_domain_type -x
```
######example
```
chcon -t user_home_t /var/www/html/index.html
```
```
chcon -t uwget http://localhost/index.html
```
######allow rules
allow roles == policy roles
```
sesearch --allow |grep httpd_content
```
```
semodule -l
semodule -d xen  //disable module
```
modules live in
```
ls -l /etc/selinux/targeted/modules/active/modules
```
copy of policy binary is in
```
/etc/selinux/targeted/policy
```
######contexts
```
/etc/selinux/targeted/contexts/files/file_contexts | grep httpd_sys_content
```








#####selinux adv
######selinux bool
```
getsebool -a | grep samba
senamage boolean -l |grep samba
setsebool samba_export_all_ro on (-P) =permanent
```
If setting policy, such as
```
chcon -t user_home_t /var/www/html/index.html
```
and wget 
```
wget http://localhost/index.html    //403 error,will record in audit
grep AVC /var/log/audit/audit.log
```
```
setmanage fcontext -at httpd_sys_content_t "/www(/.*)?"
restorecon -R -v /www
```
```
audit2allow -aM test.local
semodule -i test.local.pp
```
######permissive domains
```
semanage permissive -l
semanage permissive -a httpd_t    //add domain
semanage permissive -d httpd_t   //remove domain
semodule -d test.local
semodule -r test.local
```
#####troubleshooting
######copying and moving file
if copying, selinux properties will NOT be copied.
we can use mv command.
Or
```
cp -c
cp --preserve-context
```

######labeling filesystem
Do notgo directly from disabled to enforcing  
disabled->permissive->roboot->enforcing->reboot  

create autolabel
```
touch /.autolabel
shutdown -r
```

```
getfattr -m security.selinux -d /var/www/html/index.html
setmanage fcontext -at httpd_sys_content_t "/web(/.*)?"
```
######log files and error
```
auditd isnot running: /var/log/messages
auditd running: /var/log/audit/audit.log
```
######dont audit
```
semodule -DB  disable
semodule -B enable
```



