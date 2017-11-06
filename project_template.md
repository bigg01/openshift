$ oc adm create-bootstrap-project-template  > template.yml


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
