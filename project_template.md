$ oc adm create-bootstrap-project-template  > template.yml


```

$ oc process project-requestv12-edit-group --param-file=infileprojectgroup-p.txt -n sixprojectspaces| oc create -f -
project "proj1" created
rolebinding "system:image-pullers" created
rolebinding "system:image-builders" created
rolebinding "system:deployers" created
rolebinding "sixedit" created
rolebinding "sixeview" created

$  cat infileprojectgroup-p.txt
netzone=v12
PROJECT_NAME=proj1
PROJECT_DISPLAYNAME=proj1
PROJECT_DESCRIPTION="my pass"
PROJECT_ADMIN_USER=tkggo
PROJECT_REQUESTING_USER=tkggo
PROJECT_GROUP_EDIT=oliedit
PROJECT_GROUP_VIEW=oliview


oc get rolebinding -n proj1
NAME                    ROLE                    USERS     GROUPS                         SERVICE ACCOUNTS   SUBJECTS
sixedit                 /edit                             oliedit
sixeview                /view                             oliview
system:deployers        /system:deployer                                                 deployer
system:image-builders   /system:image-builder                                            builder
system:image-pullers    /system:image-puller              system:serviceaccounts:proj1
 

```



https://rawgit.com/openshift/openshift-logos-icon/master/demo.html


```
oc describe project proj1
Name:			proj1
Created:		7 minutes ago
Labels:			netzone=v12
Annotations:		openshift.io/description=my pass
			openshift.io/display-name=proj1
			openshift.io/requester=tkggo
			openshift.io/sa.scc.mcs=s0:c16,c0
			openshift.io/sa.scc.supplemental-groups=1000240000/10000
			openshift.io/sa.scc.uid-range=1000240000/10000
Display Name:		proj1
Description:		my pass
Status:			Active
Node Selector:		<none>
Quota:			<none>
Resource limits:	<none>

```
