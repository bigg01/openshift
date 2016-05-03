# openshift

The goal create a pod where is not dying

https://docs.openshift.org/latest/dev_guide/new_app.html
https://docs.openshift.org/latest/getting_started/administrators.html#running-in-a-docker-container

# run tty container root yum install
# oc run -i --tty temp2 --image centos:latest
Waiting for pod elk/temp2-1-gyvia to be running, status is Pending, pod ready: false

Hit enter for command prompt

[root@temp2-1-gyvia /]#
[root@temp2-1-gyvia /]#
[root@temp2-1-gyvia /]#
[root@temp2-1-gyvia /]# id
uid=0(root) gid=0(root) groups=0(root)
[root@temp2-1-gyvia /]# yum install http
Loaded plugins: fastestmirror, ovl
base                                                     | 3.6 kB     00:00
extras                                                   | 3.4 kB     00:00
updates                                                  | 3.4 kB     00:00
(1/4): extras/7/x86_64/primary_db                          | 117 kB   00:00
(2/4): base/7/x86_64/group_gz                              | 155 kB   00:00
(3/4): base/7/x86_64/primary_db                            | 5.3 MB   00:01
(4/4): updates/7/x86_64/primary_db                         | 4.1 MB   00:01
Determining fastest mirrors
 * base: mirror.switch.ch
 * extras: mirror.23media.de
 * updates: centosmirror.netcup.net
No package http available.
Error: Nothing to do
[root@temp2-1-gyvia /]# yum install httpd -y
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
 * base: mirror.switch.ch
 * extras: mirror.23media.de
 * updates: centosmirror.netcup.net
Resolving Dependencies
--> Running transaction check
---> Package httpd.x86_64 0:2.4.6-40.el7.centos will be installed
--> Processing Dependency: httpd-tools = 2.4.6-40.el7.centos for package: httpd-2.4.6-40.el7.centos.x86_64
--> Processing Dependency: system-logos >= 7.92.1-1 for package: httpd-2.4.6-40.el7.centos.x86_64
--> Processing Dependency: /etc/mime.types for package: httpd-2.4.6-40.el7.centos.x86_64
--> Processing Dependency: libaprutil-1.so.0()(64bit) for package: httpd-2.4.6-40.el7.centos.x86_64
--> Processing Dependency: libapr-1.so.0()(64bit) for package: httpd-2.4.6-40.el7.centos.x86_64
--> Running transaction check
---> Package apr.x86_64 0:1.4.8-3.el7 will be installed
---> Package apr-util.x86_64 0:1.5.2-6.el7 will be installed
---> Package centos-logos.noarch 0:70.0.6-3.el7.centos will be installed
---> Package httpd-tools.x86_64 0:2.4.6-40.el7.centos will be installed
---> Package mailcap.noarch 0:2.1.41-2.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package             Arch          Version                    Repository   Size
================================================================================
Installing:
 httpd               x86_64        2.4.6-40.el7.centos        base        2.7 M
Installing for dependencies:
 apr                 x86_64        1.4.8-3.el7                base        103 k
 apr-util            x86_64        1.5.2-6.el7                base         92 k
 centos-logos        noarch        70.0.6-3.el7.centos        base         21 M
 httpd-tools         x86_64        2.4.6-40.el7.centos        base         82 k
 mailcap             noarch        2.1.41-2.el7               base         31 k

Transaction Summary
================================================================================
Install  1 Package (+5 Dependent packages)

Total download size: 24 M
Installed size: 31 M
Downloading packages:
warning: /var/cache/yum/x86_64/7/base/packages/apr-util-1.5.2-6.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Public key for apr-util-1.5.2-6.el7.x86_64.rpm is not installed

# connect mysql port forwarding
```oc port-forward mysql-1-kabxl  3306:3306```
I0417 13:42:13.326021    2342 portforward.go:213] Forwarding from 127.0.0.1:3306 -> 3306
I0417 13:42:13.326255    2342 portforward.go:213] Forwarding from [::1]:3306 -> 3306
 vI0417 13:43:38.949544    2342 portforward.go:247] Handling connection for 3306
I0417 13:43:41.904705    2342 portforward.go:247] Handling connection for 3306
I0417 13:43:41.977789    2342 portforward.go:247] Handling connection for 3306

# new app from dockerfile
oc new-app https://github.com/bigg01/dockerfun.git --context-dir=flask/  --strategy=docker

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

oc create -f centos-base-working.json

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
#  create user
```
cat <<EOF | oc create -n test -f -
kind: ServiceAccount
apiVersion: v1
metadata:
  name: oliuser 
EOF


runAsUser:
  type: RunAsAny
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
"/tmp/kubectl-edit-lmcwy.yaml" 41L, 1137C written
error: apiVersion should not be changed
A copy of your changes has been stored to "/tmp/kubectl-edit-lmcwy.yaml"
[root@atomic-node01 origin]# oc edit scc restricted
```
<img src="https://github.com/bigg01/openshift/blob/master/ose_pod.jpg" 
alt="pod" width="340" height="180" border="10" /></a>
<img src="https://raw.githubusercontent.com/bigg01/openshift/master/ose_overview.jpg" 
alt="overview" width="340" height="180" border="10" /></a>

