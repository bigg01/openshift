##  create a groupd with permissions
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
```
