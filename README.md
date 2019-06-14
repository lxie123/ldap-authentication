# ldap-authentication
Kubernetes Authentication with LDAP - Soup to Nuts
* create namespace: kube-authentication
* Edit host section of the script 1 with environment ip addresses. ( currently you need to start the dex service to get the  cluster ip)
  * ```  "hosts": [                   
      "dex.kube-authentication.svc.cluster.local", 
      "10.102.61.151",  <-- clusterIP of Dex service
      "18.217.73.69"   <-- External IP of worker node that the dex pod is running
    ],
* run cert process scripts 1-3
* run all  ldap yaml
* run all phpldapadmin yaml
* run all gangway yaml
* modify gangway configmap
  * ``` clusterName: "GANGWAY"  <-- any name you want?
    apiServerURL: "https://18.217.73.69:6443"  <-- K8s api endpoint
    authorizeURL: "https://18.217.73.69:30010/auth" <--  Dex external endpoint (nodeport)
    tokenURL: "https://18.217.73.69:30010/token"  <--  Dex external endpoint
    clientID: "dex"
    clientSecret: "ZXhhbXBsZS1hcHAtc2VjcmV0"   <-- anything you want just need to be same string as the one you put in dex configmap
    redirectURL: "http://18.217.73.69:32000/callback"  <-- Gangway external endpoint
* Reapply gangway-configmap
* run all dex yaml
* modify dex configmap
  * ```  issuer: https://18.217.73.69:30010  <-- Dex endpoint
    storage:
      type: kubernetes
      config:
        inCluster: true
    web:
      https: 0.0.0.0:5556
      tlsCert: /etc/dex/tls/tls.crt
      tlsKey: /etc/dex/tls/tls.key
    staticClients:
    - id: dex 
      redirectURIs:
      - 'http://18.217.73.69:32000/callback'  <-- gangway endpoint
      name: 'gangway'
      secret: ZXhhbXBsZS1hcHAtc2VjcmV0  <-- same string from gangway configmap
    connectors:
    - type: ldap
      name: OpenLDAP
      id: ldap
      config:
        host: 10.100.230.194  <-- ldap service  clusterip  
* Reapply dex configmap
* Apply the API flags and load the cert see api-server-setup.txt


* Create user using phpadmin http://<Kubeadmin worker node ip>:31000
  * login:cn=admin,dc=example,dc=org / pass:admin
* Login to gangway http://<Kubeadmin worker node ip>:32000
