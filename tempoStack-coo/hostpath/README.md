Hostpath is for demo purpose only 
You also need to execute following after initializing the PV

to identitfy the uid/gid the pv needs to be owned
```
oc -n tempo get pod -l app.kubernetes.io/component=ingester -o yaml | \
  yq -ry '.items[]|.spec.securityContext.fsGroup'
``` 

to add the correct SELinux context and permissions
```
oc debug node/<nodename> 
chroot /host
semanage fcontext -a -t container_file_t '/var/hpvolumes(/.*)?'
chown <fsGroupId>:<fsGroupId> /var/hpvolumes 
```
