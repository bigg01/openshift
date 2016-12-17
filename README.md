# openshif

`git clone https://github.com/kamon-io/docker-grafana-graphite.git
Cloning into 'docker-grafana-graphite'...
remote: Counting objects: 262, done.
remote: Total 262 (delta 0), reused 0 (delta 0), pack-reused 262
Receiving objects: 100% (262/262), 75.97 KiB | 0 bytes/s, done.
Resolving deltas: 100% (113/113), done.
Checking connectivity... done.
 guo  ~  grafana-compose  $   oc import docker-compose -f ./docker-compose.yml -o yaml
error: open /Users/guo/grafana-compose/Dockerfile: no such file or directory
 guo  ~  grafana-compose  $  ls
docker-compose.yml      docker-grafana-graphite
 guo  ~  grafana-compose  $  cd docker-grafana-graphite/
 guo  ~  grafana-compose  docker-grafana-graphite   master  $   oc import docker-compose -f ./docker-compose.yml -o yaml
apiVersion: v1
items:
- apiVersion: v1
  kind: ImageStream
  metadata:
    creationTimestamp: null
    name: grafana_graphite
  spec:
    tags:
    - annotations:
        openshift.io/imported-from: ubuntu:14.04
      from:
        kind: DockerImage
        name: ubuntu:14.04
      generation: null
      importPolicy: {}
      name: from
  status:
    dockerImageRepository: ""
- apiVersion: v1
  kind: BuildConfig
  metadata:
    creationTimestamp: null
    name: grafana_graphite
  spec:
    nodeSelector: null
    output:
      to:
        kind: ImageStreamTag
        name: grafana_graphite:latest
    postCommit: {}
    resources: {}
    source:
      contextDir: .
      git:
        ref: master
        uri: https://github.com/kamon-io/docker-grafana-graphite.git
      type: Git
    strategy:
      dockerStrategy:
        from:
          kind: ImageStreamTag
          name: grafana_graphite:from
      type: Docker
    triggers:
    - github:
        secret: MRaAp3h2ynQzm5ewavfn
      type: GitHub
    - generic:
        secret: P9PCFl9bawP7DmPBLjjK
      type: Generic
    - type: ConfigChange
    - imageChange: {}
      type: ImageChange
  status:
    lastVersion: 0
- apiVersion: v1
  kind: ImageStream
  metadata:
    creationTimestamp: null
    name: grafanagraphite
  spec:
    tags:
    - annotations:
        openshift.io/imported-from: kamon/grafana_graphite
      from:
        kind: DockerImage
        name: kamon/grafana_graphite
      generation: null
      importPolicy: {}
      name: latest
  status:
    dockerImageRepository: ""
- apiVersion: v1
  kind: DeploymentConfig
  metadata:
    creationTimestamp: null
    name: grafanagraphite
  spec:
    replicas: 1
    selector:
      deploymentconfig: grafanagraphite
    strategy:
      resources: {}
    template:
      metadata:
        creationTimestamp: null
        labels:
          deploymentconfig: grafanagraphite
      spec:
        containers:
        - image: kamon/grafana_graphite
          name: kamon-grafana-dashboard
          ports:
          - containerPort: 80
          - containerPort: 81
          - containerPort: 8126
          resources: {}
          volumeMounts:
          - mountPath: /opt/grafana/data
            name: dir-1
          - mountPath: /opt/graphite/storage/log
            name: dir-2
          - mountPath: /opt/graphite/storage/whisper
            name: dir-3
        volumes:
        - emptyDir: {}
          name: dir-1
        - emptyDir: {}
          name: dir-2
        - emptyDir: {}
          name: dir-3
    test: false
    triggers:
    - type: ConfigChange
    - imageChangeParams:
        automatic: true
        containerNames:
        - grafanagraphite
        from:
          kind: ImageStreamTag
          name: grafanagraphite:latest
      type: ImageChange
  status: {}
- apiVersion: v1
  kind: Service
  metadata:
    creationTimestamp: null
    name: grafanagraphite
  spec:
    ports:
    - name: 80-tcp
      port: 80
      targetPort: 80
    - name: 81-tcp
      port: 81
      targetPort: 81
    - name: 8126-tcp
      port: 8126
      targetPort: 8126
    selector:
      deploymentconfig: grafanagraphite
  status:
    loadBalancer: {}
kind: List
metadata: {}`
The goal create a pod where is not dying

https://docs.openshift.org/latest/dev_guide/new_app.html
https://docs.openshift.org/latest/getting_started/administrators.html#running-in-a-docker-container

# template get
`oc get pod  minion-1-5a9p6 -o go-template={{.status.podIP}}`

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

# rm -rf /run/httpd/* /tmp/httpd*
[root@temp2-1-gyvia /]#
[root@temp2-1-gyvia /]# exec /usr/sbin/apachectl -DFOREGROUND

# # oc port-forward temp2-1-gyvia 8083:80
I0503 21:42:05.607473    2524 portforward.go:213] Forwarding from 127.0.0.1:8083 -> 80
I0503 21:42:05.607612    2524 portforward.go:213] Forwarding from [::1]:8083 -> 80
I0503 21:42:19.696162    2524 portforward.go:247] Handling connection for 8083
I0503 21:42:19.696289    2524 portforward.go:247] Handling connection for 8083

open browser 127.0.0.1:8083


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


# run as any user
https://blog.openshift.com/getting-any-docker-image-running-in-your-own-openshift-cluster/

run any user
oadm policy add-scc-to-user anyuid -z playground
oc edit scc anyuid

`
users:
- system:serviceaccount:hexboard:instant-container
- system:serviceaccount:admin:instant-container
- system:serviceaccount:playground:default
`
oc status
In project playground on server https://xxx:8443

oc run --restart=Never --attach --stdin --tty --image registry.access.redhat.com/rhel7 olitest  /bin/sh

 oc run -i --tty temp2 --image registry.access.redhat.com/rhel7

Waiting for pod playground/temp2-1-gognv to be running, status is Pending, pod ready: false





[root@temp2-1-gognv /]# 
[root@temp2-1-gognv /]# 
[root@temp2-1-gognv /]# 
[root@temp2-1-gognv /]# 
[root@temp2-1-gognv /]# 
[root@temp2-1-gognv /]# id
uid=0(root) gid=0(root) groups=0(root)

```
oc get pods -o json 2>/dev/null |jq -r '.items[].status.podIP'
172.17.0.3
172.17.0.3
172.17.0.7
172.17.0.3
172.17.0.3
```
