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

## Cluster Issuer 
```
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    # You must replace this email address with your own.
    # Let's Encrypt will use this to contact you about expiring
    # certificates, and issues related to your account.
    email: dslee1371@gmail.com
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      # Secret resource that will be used to store the account's private key.
      name: acme-issuer-account-key-2
    # Add a single challenge solver, HTTP01 using nginx
    solvers:
    - http01:
        ingress:
          class: nginx
```

## Certificate 
```
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: acme-cert
spec:
  secretName: acme-cert-tls
  dnsNames:
  - nginx-unprivileged-lab1-test1.apps.oss2.fu.igotit.co.kr
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```
 
