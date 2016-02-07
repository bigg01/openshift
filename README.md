# openshift

The goal create a pod where is not dying

# Create pod
oc create -f ./oli-pod.json

# delete pod
oc delete pod olipod

# get pods
oc get pod
NAME                      READY     STATUS      RESTARTS   AGE
hello-openshift-1-rm2is   1/1       Running     0          1h
olicentos                 0/1       Completed   0          19m
olipod                    1/1       Running     0          5m

# rsh into pod
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
