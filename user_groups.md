##  create a groupd with permissions
3.6 
https://docs.openshift.com/container-platform/3.6/admin_solutions/user_role_mgmt.html#view-roles-users-project
https://docs.openshift.com/container-platform/3.6/architecture/additional_concepts/authorization.html

```sh
# new group
oc adm groups new oligroup
# add users to group
oc adm groups add-users oligroup developer

oc describe group oligroup

# add cluster-admin remove from user developer
oc adm policy remove-cluster-role-to-user cluster-admin developer

# add cluster-admin to user group 
oc adm policy add-cluster-role-to-group cluster-admin oligroup
oc describe group oligroup
oc get  group oligroup -o yaml



oc adm policy remove-cluster-role-from-user cluster-admin developer


## roles
oc export  ClusterRole admin > brole2.yml

apiVersion: v1
kind: ClusterRole
metadata:
  annotations:
    openshift.io/description: A user that has edit rights within the project and can
      change the project's membership.
  creationTimestamp: null
  name: admin-noremote
rules:
- apiGroups:
  - ""
  attributeRestrictions: null
  resources:
  - pods
  - pods/attach # terminal access rsh
  - pods/exec
  - pods/portforward # port-forward
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


```
```
oc export  ClusterRole edit > edit.yml
```

```
apiVersion: v1
kind: ClusterRole
metadata:
  annotations:
    openshift.io/description: A user that can create and edit most objects in a project,
      but can not update the project's membership.
  creationTimestamp: null
  name: edit
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
  - jobs
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
  - build.openshift.io
  - ""
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
  - build.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - builds/log
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - build.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - buildconfigs/instantiate
  - buildconfigs/instantiatebinary
  - builds/clone
  verbs:
  - create
- apiGroups:
  - build.openshift.io
  - ""
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
  - edit
  - view
- apiGroups:
  - apps.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - deploymentconfigs
  - deploymentconfigs/scale
  - generatedeploymentconfigs
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
  - apps.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - deploymentconfigrollbacks
  - deploymentconfigs/instantiate
  - deploymentconfigs/rollback
  verbs:
  - create
- apiGroups:
  - apps.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - deploymentconfigs/log
  - deploymentconfigs/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - image.openshift.io
  - ""
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
  - image.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - imagestreams/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - image.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - imagestreams/layers
  verbs:
  - get
  - update
- apiGroups:
  - image.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - imagestreamimports
  verbs:
  - create
- apiGroups:
  - project.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - projects
  verbs:
  - get
- apiGroups:
  - quota.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - appliedclusterresourcequotas
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - route.openshift.io
  - ""
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
  - route.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - routes/custom-host
  verbs:
  - create
- apiGroups:
  - route.openshift.io
  - ""
  attributeRestrictions: null
  resources:
  - routes/status
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - template.openshift.io
  - ""
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
  - build.openshift.io
  - ""
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
```

