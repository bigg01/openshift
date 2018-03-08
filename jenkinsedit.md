```sh
$ oc get ClusterRole  |grep admin
$ oc export ClusterRole admin > jenkins-admin.yml
$ vi jenkins-admin.yml
$ oc export ClusterRole admin > jenkins-admin-orig.yml
 
diff -u jenkins-admin-orig.yml jenkins-admin.yml
--- jenkins-admin-orig.yml	2018-03-08 20:25:52.000000000 +0100
+++ jenkins-admin.yml	2018-03-08 20:25:42.000000000 +0100
@@ -6,7 +6,7 @@
       change the project's membership.
     openshift.io/reconcile-protect: "false"
   creationTimestamp: null
-  name: admin
+  name: jenkins-admin
 rules:
 - apiGroups:
   - ""
@@ -362,10 +362,7 @@
   resources:
   - projects
   verbs:
-  - delete
   - get
-  - patch
-  - update
 - apiGroups:
   - ""
   - quota.openshift.io

```
