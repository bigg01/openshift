# openshift

The goal create a pod where is not dying

![alt tag](https://github.com/bigg01/openshift/raw/master/images/ose_overview.tiff)


# Create pod
oc create -f ./oli-pod.json

# delete pod
oc delete pod olipod

# get pods
```
oc get pod
NAME                      READY     STATUS      RESTARTS   AGE
hello-openshift-1-rm2is   1/1       Running     0          1h
olicentos                 0/1       Completed   0          19m
olipod                    1/1       Running     0          5m
```
# rsh into pod
```
oc rsh olipod
bash-4.2$
bash-4.2$ ls
anaconda-post.log  dev	home  lib64	  media  opt   root  sbin  sys	usr
bin		   etc	lib   lost+found  mnt	 proc  run   srv   tmp	var
bash-4.2$ ps
  PID TTY          TIME CMD
  381 ?        00:00:00 bash
  395 ?        00:00:00 ps
bash-4.2$
```
# rpm install in openshift?

```
oc rsh olipod
bash-4.2$ yum install httpd -y
Loaded plugins: fastestmirror, ovl
ovl: Error while doing RPMdb copy-up:
[Errno 13] Permission denied: '/var/lib/rpm/.dbenv.lock'


Error making cache directory: /var/cache/yum/x86_64/7/base error was: [Errno 13] Permission denied: '/var/cache/yum/x86_64'
bash-4.2$
```
Templates
https://docs.openshift.org/latest/dev_guide/templates.html#uploading-a-template
