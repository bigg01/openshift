```sh
$ oc get ClusterRole  |grep admin
$ oc export ClusterRole admin > jenkins-admin.yml
$ vi jenkins-admin.yml
$ oc export ClusterRole admin > jenkins-admin-orig.yml
 
diff -u jenkins-admin-orig.yml jenkins-admin.yml
--- jenkins-admin-orig.yml	2018-03-08 20:25:52.000000000 +0100
+++ jenkins-admin.yml	2018-03-08 20:47:04.000000000 +0100
@@ -6,7 +6,7 @@
       change the project's membership.
     openshift.io/reconcile-protect: "false"
   creationTimestamp: null
-  name: admin
+  name: jenkins-admin
 rules:
 - apiGroups:
   - ""
@@ -158,13 +158,9 @@
   - rolebindings
   - roles
   verbs:
-  - create
-  - delete
-  - deletecollection
   - get
   - list
   - patch
-  - update
   - watch
 - apiGroups:
   - rbac.authorization.k8s.io
@@ -173,13 +169,8 @@
   - rolebindings
   - roles
   verbs:
-  - create
-  - delete
-  - deletecollection
   - get
   - list
-  - patch
-  - update
   - watch
 - apiGroups:
   - ""
@@ -362,10 +353,7 @@
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


```yaml
apiVersion: authorization.openshift.io/v1
groupNames: null
kind: RoleBinding
metadata:
  creationTimestamp: null
  name: sixedit
roleRef:
  name: jenkins-admin
```


```
oc get rolebindings sixedit -o yaml
apiVersion: v1
groupNames: null
kind: RoleBinding
metadata:
  creationTimestamp: 2018-03-08T19:36:42Z
  name: sixedit
  namespace: myproject
  resourceVersion: "2821"
  selfLink: /oapi/v1/namespaces/myproject/rolebindings/sixedit
  uid: 0767adaa-2308-11e8-86c5-025000000001
roleRef:
  name: jenkins-admin
subjects:
- kind: User
  name: developer
userNames:
- developer
 guo  ~  $  oc get rolebindings
NAME                    ROLE                    USERS       GROUPS                             SERVICE ACCOUNTS   SUBJECTS
admin                   /admin
sixedit                 /jenkins-admin          developer
system:deployers        /system:deployer                                                       deployer
system:image-builders   /system:image-builder                                                  builder
system:image-pullers    /system:image-puller                system:serviceaccounts:myproject
```

```yaml
#cat jenkins-admin.yml
apiVersion: v1
kind: ClusterRole
metadata:
  annotations:
    openshift.io/description: A user that has edit rights within the project and can
      change the project's membership.
    openshift.io/reconcile-protect: "false"
  creationTimestamp: null
  name: jenkins-admin
rules:
- apiGroups:
  - ""
  attributeRestrictions: null
  resources:
  - pods
  - pods/attach
  - pods/exec
  - pods/portforward
  - pods/proxy
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  attributeRestrictions: null
  resources:
  - configmaps
  - endpoints
  - persistentvolumeclaims
  - replicationcontrollers
  - replicationcontrollers/scale
  - secrets
  - serviceaccounts
  - services
  - services/proxy
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  attributeRestrictions: null
  resources:
  - bindings
  - events
  - limitranges
  - namespaces
  - namespaces/status
  - pods/log
  - pods/status
  - replicationcontrollers/status
  - resourcequotas
  - resourcequotas/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  attributeRestrictions: null
  resources:
  - serviceaccounts
  verbs:
  - impersonate
- apiGroups:
  - autoscaling
  attributeRestrictions: null
  resources:
  - horizontalpodautoscalers
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - batch
  attributeRestrictions: null
  resources:
  - cronjobs
  - jobs
  - scheduledjobs
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - extensions
  attributeRestrictions: null
  resources:
  - deployments
  - deployments/rollback
  - deployments/scale
  - horizontalpodautoscalers
  - networkpolicies
  - replicasets
  - replicasets/scale
  - replicationcontrollers/scale
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - extensions
  attributeRestrictions: null
  resources:
  - daemonsets
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - apps
  attributeRestrictions: null
  resources:
  - deployments
  - deployments/scale
  - deployments/status
  - statefulsets
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  - authorization.openshift.io
  attributeRestrictions: null
  resources:
  - rolebindings
  - roles
  verbs:
  - get
  - list
  - patch
  - watch
- apiGroups:
  - rbac.authorization.k8s.io
  attributeRestrictions: null
  resources:
  - rolebindings
  - roles
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - authorization.openshift.io
  attributeRestrictions: null
  resources:
  - localresourceaccessreviews
  - localsubjectaccessreviews
  - subjectrulesreviews
  verbs:
  - create
- apiGroups:
  - ""
  - security.openshift.io
  attributeRestrictions: null
  resources:
  - podsecuritypolicyreviews
  - podsecuritypolicyselfsubjectreviews
  - podsecuritypolicysubjectreviews
  verbs:
  - create
- apiGroups:
  - authorization.k8s.io
  attributeRestrictions: null
  resources:
  - localsubjectaccessreviews
  verbs:
  - create
- apiGroups:
  - ""
  - authorization.openshift.io
  attributeRestrictions: null
  resources:
  - rolebindingrestrictions
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - build.openshift.io
  attributeRestrictions: null
  resources:
  - buildconfigs
  - buildconfigs/webhooks
  - builds
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  - build.openshift.io
  attributeRestrictions: null
  resources:
  - builds/log
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - build.openshift.io
  attributeRestrictions: null
  resources:
  - buildconfigs/instantiate
  - buildconfigs/instantiatebinary
  - builds/clone
  verbs:
  - create
- apiGroups:
  - ""
  - build.openshift.io
  attributeRestrictions: null
  resources:
  - builds/details
  verbs:
  - update
- apiGroups:
  - build.openshift.io
  attributeRestrictions: null
  resources:
  - jenkins
  verbs:
  - admin
  - edit
  - view
- apiGroups:
  - ""
  - apps.openshift.io
  attributeRestrictions: null
  resources:
  - deploymentconfigs
  - deploymentconfigs/scale
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  - apps.openshift.io
  attributeRestrictions: null
  resources:
  - deploymentconfigrollbacks
  - deploymentconfigs/instantiate
  - deploymentconfigs/rollback
  verbs:
  - create
- apiGroups:
  - ""
  - apps.openshift.io
  attributeRestrictions: null
  resources:
  - deploymentconfigs/log
  - deploymentconfigs/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - image.openshift.io
  attributeRestrictions: null
  resources:
  - imagestreamimages
  - imagestreammappings
  - imagestreams
  - imagestreams/secrets
  - imagestreamtags
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  - image.openshift.io
  attributeRestrictions: null
  resources:
  - imagestreams/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - image.openshift.io
  attributeRestrictions: null
  resources:
  - imagestreams/layers
  verbs:
  - get
  - update
- apiGroups:
  - ""
  - image.openshift.io
  attributeRestrictions: null
  resources:
  - imagestreamimports
  verbs:
  - create
- apiGroups:
  - ""
  - project.openshift.io
  attributeRestrictions: null
  resources:
  - projects
  verbs:
  - get
- apiGroups:
  - ""
  - quota.openshift.io
  attributeRestrictions: null
  resources:
  - appliedclusterresourcequotas
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - route.openshift.io
  attributeRestrictions: null
  resources:
  - routes
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  - route.openshift.io
  attributeRestrictions: null
  resources:
  - routes/custom-host
  verbs:
  - create
- apiGroups:
  - ""
  - route.openshift.io
  attributeRestrictions: null
  resources:
  - routes/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - route.openshift.io
  attributeRestrictions: null
  resources:
  - routes/status
  verbs:
  - update
- apiGroups:
  - ""
  - template.openshift.io
  attributeRestrictions: null
  resources:
  - processedtemplates
  - templateconfigs
  - templateinstances
  - templates
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  - build.openshift.io
  attributeRestrictions: null
  resources:
  - buildlogs
  verbs:
  - create
  - delete
  - deletecollection
  - get
  - list
  - patch
  - update
  - watch
- apiGroups:
  - ""
  attributeRestrictions: null
  resources:
  - resourcequotausages
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  - authorization.openshift.io
  attributeRestrictions: null
  resources:
  - resourceaccessreviews
  - subjectaccessreviews
  verbs:
  - create

```


```yaml
apiVersion: v1
groupNames: null
kind: RoleBinding
metadata:
  name: jenkins_edit
roleRef:
  name: edit
subjects:
- kind: ServiceAccount
  name: jenkins
```
NAME                    ROLE                    USERS       GROUPS                             SERVICE ACCOUNTS   SUBJECTS
