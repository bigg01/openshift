$ oc adm create-bootstrap-project-template  > template.yml


```

oc process project-requestv12-edit-group --param-file=infileprojectgroup-p.txt -n sixprojectspaces| oc create -f -
project "proj1" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "sixedit" created
rolebinding "sixeview" created
 guo  ~  $  cat infileprojectgroup-p.txt
netzone=v12
PROJECT_NAME=proj1
PROJECT_DISPLAYNAME=proj1
PROJECT_DESCRIPTION="my pass"
PROJECT_ADMIN_USER=tkggo
PROJECT_REQUESTING_USER=tkggo
PROJECT_GROUP_EDIT=oliedit
PROJECT_GROUP_VIEW=oliview


 
 

```



https://rawgit.com/openshift/openshift-logos-icon/master/demo.html

$ oc process  project-requestv12 --parameters
NAME                      DESCRIPTION         GENERATOR           VALUE
PROJECT_NAME
PROJECT_DISPLAYNAME
PROJECT_DESCRIPTION
PROJECT_ADMIN_USER
PROJECT_REQUESTING_USER



```

oc process  project-requestv12 --parameters
NAME                      DESCRIPTION         GENERATOR           VALUE
PROJECT_NAME
PROJECT_DISPLAYNAME
PROJECT_DESCRIPTION
PROJECT_ADMIN_USER
PROJECT_REQUESTING_USER
 guo  ~  $  oc process  project-requestv12
 guo  ~  $  vi infileproject.txt
 guo  ~  $  cat infileproject.txt
PROJECT_NAME="olig"
PROJECT_DISPLAYNAME="hhh"
PROJECT_DESCRIPTION="desc"
PROJECT_ADMIN_USER="tkggo"
PROJECT_REQUESTING_USER="tkggo"
 guo  ~  $  oc process  project-requestv12 --param-file=infileproject.txt
{
    "kind": "List",
    "apiVersion": "v1",
    "metadata": {},
    "items": [
        {
            "apiVersion": "project.openshift.io/v1",
            "kind": "Project",
            "metadata": {
                "annotations": {
                    "openshift.io/description": "desc",
                    "openshift.io/display-name": "hhh",
                    "openshift.io/requester": "tkggo"
                },
                "creationTimestamp": null,
                "name": "olig"
            },
            "spec": {},
            "status": {}
        },
        {
            "apiVersion": "authorization.openshift.io/v1",
            "groupNames": [
                "system:serviceaccounts:olig"
            ],
            "kind": "RoleBinding",
            "metadata": {
                "creationTimestamp": null,
                "name": "system:image-pullers",
                "namespace": "olig"
            },
            "roleRef": {
                "name": "system:image-puller"
            },
            "subjects": [
                {
                    "kind": "SystemGroup",
                    "name": "system:serviceaccounts:olig"
                }
            ],
            "userNames": null
        },
        {
            "apiVersion": "authorization.openshift.io/v1",
            "groupNames": null,
            "kind": "RoleBinding",
            "metadata": {
                "creationTimestamp": null,
                "name": "system:image-builders",
                "namespace": "olig"
            },
            "roleRef": {
                "name": "system:image-builder"
            },
            "subjects": [
                {
                    "kind": "ServiceAccount",
                    "name": "builder"
                }
            ],
            "userNames": [
                "system:serviceaccount:olig:builder"
            ]
        },
        {
            "apiVersion": "authorization.openshift.io/v1",
            "groupNames": null,
            "kind": "RoleBinding",
            "metadata": {
                "creationTimestamp": null,
                "name": "system:deployers",
                "namespace": "olig"
            },
            "roleRef": {
                "name": "system:deployer"
            },
            "subjects": [
                {
                    "kind": "ServiceAccount",
                    "name": "deployer"
                }
            ],
            "userNames": [
                "system:serviceaccount:olig:deployer"
            ]
        },
        {
            "apiVersion": "authorization.openshift.io/v1",
            "groupNames": null,
            "kind": "RoleBinding",
            "metadata": {
                "creationTimestamp": null,
                "name": "admin",
                "namespace": "olig"
            },
            "roleRef": {
                "name": "admin"
            },
            "subjects": [
                {
                    "kind": "User",
                    "name": "tkggo"
                }
            ],
            "userNames": [
                "tkggo"
            ]
        }
    ]
}



oc process  project-requestv12 --param-file=infileproject.txt | oc create -f -
project "olig" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "admin" created
```
$ oc process  project-requestv12 --param-file=infileproject.txt -n sixprojectspaces| oc create -f -
project "olig" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "admin" created


$ oc process project-requestv12-edit --param-file=infileproject.txt -n sixprojectspaces| oc create -f -
project "olig" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "edit" created



oc adm groups new oliedit
  584  oc adm groups add-users oliedit tkggo-e
  585  oc describe groups  oliedit
  586  history
 guo  ~  $  oc describe groups  oliedit
Name:		oliedit
Created:	2 minutes ago
Labels:		<none>
Annotations:	<none>
Users:		tkggo-e



```sh
oc get rolebindings -n olig
NAME                    ROLE                    USERS     GROUPS                        SERVICE ACCOUNTS   SUBJECTS
basic-user              /basic-user             tkggo-b
edit                    /edit                             oliedit
system:deployers        /system:deployer                                                deployer
system:image-builders   /system:image-builder                                           builder
system:image-pullers    /system:image-puller              system:serviceaccounts:olig
view                    /view                   tkggo-v
 guo  ~  $  oc get -o yaml rolebindings edit
apiVersion: v1
groupNames:
- oliedit
kind: RoleBinding
metadata:
  creationTimestamp: 2017-11-06T20:53:40Z
  name: edit
  namespace: olig
  resourceVersion: "3530"
  selfLink: /oapi/v1/namespaces/olig/rolebindings/edit
  uid: 91b85e08-c334-11e7-a94e-12648b8d89aa
roleRef:
  name: edit
subjects:
- kind: Group
  name: oliedit
userNames: null
```



```
 oc process project-requestv12-edit-group --param-file=infileprojectgroup.txt -n sixprojectspaces| oc create -f -
project "oliggroup" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "edit" created
 guo  ~  $  oc get rolebindings -n oliggroup
NAME                    ROLE                    USERS     GROUPS                             SERVICE ACCOUNTS   SUBJECTS
edit                    /edit                             oliedit
system:deployers        /system:deployer                                                     deployer
system:image-builders   /system:image-builder                                                builder
system:image-pullers    /system:image-puller              system:serviceaccounts:oliggroup
 guo  ~  $  cat infileprojectgroup.txt
PROJECT_NAME="oliggroup"
PROJECT_DISPLAYNAME="oliggroup"
PROJECT_DESCRIPTION="desc"
PROJECT_ADMIN_USER="tkggo"
PROJECT_REQUESTING_USER="tkggo"
PROJECT_ADMIN_GROUP="oliedit"
```


```sh

oc process project-requestv12-edit-group --param-file=infileprojectgroup-p.txt -n sixprojectspaces| oc create -f -
project "proj1" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "sixedit" created
 guo  ~  $  cat infileprojectgroup-p.txt
netzone=v12
PROJECT_NAME=proj1
PROJECT_DISPLAYNAME=proj1
PROJECT_DESCRIPTION="my pass"
PROJECT_ADMIN_USER=tkggo
PROJECT_REQUESTING_USER=tkggo
PROJECT_ADMIN_GROUP=oliedit
PROJECT_ADMIN_TYPE=edit
```

PROJECT_DESCRIPTION="desc"`^
PROJECT_DESCRIPTION="desc"
