# ldap-authentication
Kubernetes Authentication with LDAP - Soup to Nuts
* create namespace: kube-authentication
* run cert process scripts 1-3
* run ldap yaml
* run phpldapadmin yaml
* run gangway yaml
* modify gangway configmap
* run dex yaml
* modify dex configmap

* Create user using phpadmin http://<Kubeadmin worker node ip>:31000
* Login to gangway http://<Kubeadmin worker node ip>:32000
