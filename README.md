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
# Templates
https://docs.openshift.org/latest/dev_guide/templates.html#uploading-a-template
YAML
```
oc export pod/olipod --as-template=standalone_centos > standalone_centos.json
```

JSON
```
oc export pod/olipod --as-template=standalone_centos -o json> standalone_centos.json
```
#create template
```
oc create -f standalone_centos_temp.json
template "standalonecentos" created

```
# run user as 0(root)
```
Cannot create pod "olipod-1". pods "olipod-1" is forbidden: unable to validate against any security context constraint: [securityContext.runAsUser: Invalid value: 0: UID on container oli-pod does not match required range. Found 0, required min: 1000050000 max: 1000059999].
```
# SCC
https://docs.openshift.com/enterprise/3.0/admin_guide/manage_scc.html
```
 oc get scc
NAME               PRIV      CAPS      HOSTDIR   SELINUX     RUNASUSER          FSGROUP    SUPGROUP   PRIORITY
anyuid             false     []        false     MustRunAs   RunAsAny           RunAsAny   RunAsAny   10
hostaccess         false     []        true      MustRunAs   MustRunAsRange     RunAsAny   RunAsAny   <none>
hostmount-anyuid   false     []        true      MustRunAs   RunAsAny           RunAsAny   RunAsAny   <none>
nonroot            false     []        false     MustRunAs   MustRunAsNonRoot   RunAsAny   RunAsAny   <none>
privileged         true      []        true      RunAsAny    RunAsAny           RunAsAny   RunAsAny   <none>
restricted         false     []        false     MustRunAs   MustRunAsRange     RunAsAny   RunAsAny   <none>
```

# create SCC
```
oc export scc privileged --as-template='olisecurity' -o json > olisecurity.json
```

# selinux container
```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    openshift.io/scc: privileged
spec:
  containers:
    securityContext:
      privileged: false
      runAsUser: 0
```

```
  "privileged": false,
                            "seLinuxOptions": {
                                "level": "s0:c7,c4"
                            },
                            "runAsUser": 1000050000
```
```
 "securityContext": {
                            "capabilities": {
                                "drop": [
                                    "KILL",
                                    "MKNOD",
                                    "SETGID",
                                    "SETUID",
                                    "SYS_CHROOT"
                                ]
                            },
```
