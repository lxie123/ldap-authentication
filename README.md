# ldap-authentication
Kubernetes Authentication with LDAP - Soup to Nuts
* create namespace: kube-authentication
* Edit host section of the script 1 with environment ip addresses. ( currently you need to start the dex service to get the  cluster ip)
```  "hosts": [                   
    "dex.kube-authentication.svc.cluster.local", 
    "10.102.61.151",  (clusterIP of Dex service)
    "18.217.73.69"   (External IP of worker node that the dex pod is running)
  ], ```

* run cert process scripts 1-3
* run all  ldap yaml
* run all phpldapadmin yaml
* run all gangway yaml
* modify gangway configmap
* Reapply gangway-configmap
* run all dex yaml
* modify dex configmap
* Reapply dex configmap
* Apply the API flags and load the cert see api-server-setup.txt


* Create user using phpadmin http://<Kubeadmin worker node ip>:31000
  * login:cn=admin,dc=example,dc=org / pass:admin
* Login to gangway http://<Kubeadmin worker node ip>:32000
