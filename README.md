# Cert-manager

## Resources

kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges 

## References

- https://cert-manager.io/docs/
- https://www.dogtagpki.org/wiki/OpenShift_cert-manager  - fail
- https://github.com/futuregen-icp/public/blob/main/cert_manger/Cert_manager%20install.md - git
- https://github.com/tnozicka/openshift-acme - openshit (ing) Keith Tenzer
- https://ddii.dev/kubernetes/cert-manager/# - refer example
- https://www.openshift.com/blog/self-serviced-end-to-end-encryption-for-kubernetes-applications-part-2-a-practical-example - refer example openshif
- https://hub.docker.com/r/nginxinc/nginx-unprivileged - nginx unprivileged

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
## Installation
```
1, install cert-manager
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.2.0/cert-manager.crds.yaml

helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.2.0 \

2, install cert-manager-webhook-oci
/home/TE_Smartworks/test/qsluo/cert-manager-webhook-oci-develop
helm install --namespace cert-manager cert-manager-webhook-oci deploy/cert-manager-webhook-oci



3, create  ClusterIssuer
cd /home/TE_Smartworks/test/qsluo
k apply -f ClusterIssuer_webhook.yaml

kubectl get ClusterIssuers
NAME                                             READY   AGE
clusterissuer.cert-manager.io/letsencrypt-prod   True    6h50m
##### 상태 곡 True  돼야 다음 진행 오늘 밤 걸어주고 다음날 확인 

4, create Certificate
cd /home/TE_Smartworks/test/qsluo
k apply -f Certificate_prod.yaml

challenge 상태가 성공 해야 함 order pending 날수도 있음 이떄 acem 변경 http01 
방식(ClusterIssuer_http_prod.yaml) 이떄는 challenge 안하고 그냥 order 만 생성
```
