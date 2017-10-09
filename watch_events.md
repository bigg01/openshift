# sbb event forwarder
https://github.com/oscp/openshift-eventforwarder/blob/master/main.go

$ oc get events --show-all --all-namespaces --watch | ccze

$ oc get events --show-all -n myproject --watch | ccze


```
017-10-09 10:24:43 +0200 CEST   2017-10-09 10:24:43 +0200 CEST   1         redis            DeploymentConfig                                      Normal    DeploymentCreated       deploymentconfig-controller   Created
new replication controller "redis-1" for version 1 
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1-xhn48   Pod                 Normal    Scheduled   default-scheduler   Successfully assigned redis-1-xhn48 to localhost 
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1   ReplicationController             Normal    SuccessfulCreate   replication-controller   Created pod: redis-1-xhn48 
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1-9wq3f   Pod       spec.containers{redis}   Normal    Killing   kubelet, localhost   Killing container with id docker://redis:Need to ki
ll Pod 
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1-xhn48   Pod                 Normal    SuccessfulMountVolume   kubelet, localhost   MountVolume.SetUp succeeded for volume "redis-volume
-1"  
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1-xhn48   Pod                 Normal    SuccessfulMountVolume   kubelet, localhost   MountVolume.SetUp succeeded for volume "default-toke
n-vg823"  
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1-xhn48   Pod       spec.containers{redis}   Normal    Pulled    kubelet, localhost   Container image "centos/redis-32-centos7@sha256:590
92d7b73976ae135d12a578e2c82b35dafb8ab4141cc1dc8d8473f27a6fa2b" already present on machine 
2017-10-09 10:28:42 +0200 CEST   2017-10-09 10:28:42 +0200 CEST   1         redis-1-xhn48   Pod       spec.containers{redis}   Normal    Created   kubelet, localhost   Created container 
2017-10-09 10:28:43 +0200 CEST   2017-10-09 10:28:43 +0200 CEST   1         redis-1-xhn48   Pod       spec.containers{redis}   Normal    Started   kubelet, localhost   Started container 

```
