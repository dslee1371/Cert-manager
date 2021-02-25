# Cert-manager

## Resources

kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges 

## References

https://cert-manager.io/docs/
https://www.dogtagpki.org/wiki/OpenShift_cert-manager  - fail
https://github.com/futuregen-icp/public/blob/main/cert_manger/Cert_manager%20install.md - git

## Lab1 - Install cert manager with openshift

```
## Install cert manager
oc apply -f https://github.com/jetstack/cert-manager/releases/download/v1.2.0/cert-manager.yaml

## Creating ACME Issuer

```

## Work logs 

1. Install Cert manager 
2. Create ACME Issuer 
	1. true
3. Create ACME Certificate
	1. check certificate false
	2. check certificaterequest false
	3. check order false 
