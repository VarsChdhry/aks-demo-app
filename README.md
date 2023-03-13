# aks-demo-app
Simple diagram how deployment communicate with clusterIP which exposes internal IP ,
target port 80 mapped on container port 80, where traffic is routed via Ingress. 

Ingress controller have SSL/TLS certificate assigned by LetsEncrypt cert manager. 


<img width="870" alt="image" src="https://user-images.githubusercontent.com/67351893/224800835-dc831c4a-a94f-4c25-abda-28883c16cb36.png">






Deployment of Service, Pod and Ingress are done using Azure DevOps Pipeline.
https://dev.azure.com/varshachaudhary325/VNDA/_build?view=folders 
I have made VNDA project public for couple of days. 
and it's YAML is here : https://github.com/VarsChdhry/aks-demo-app/blob/main/azure-pipelines.yml

<img width="817" alt="image" src="https://user-images.githubusercontent.com/67351893/224803370-055afb28-c45c-4d03-a938-1e9d8b48a3f6.png">



There are other components that are deployed via Helm Charts like Ingress Controller, Cert-Manager, show in below steps. 

```
# Create a namespace for ingress resources
kubectl create namespace ingress-basic

# Add the Helm repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

# Use Helm to deploy an NGINX ingress controller
helm install ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-basic \
    --set controller.replicaCount=1
```

```
# Label the cert-manager namespace to disable resource validation
kubectl label namespace ingress-basic cert-manager.io/disable-validation=true
# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io
# Update your local Helm chart repository cache
helm repo update
# Install CRDs with kubectl
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.7.1/cert-manager.crds.yaml
# Install the cert-manager Helm chart
helm install cert-manager jetstack/cert-manager \
  --namespace ingress-basic \
  --version v1.7.1
```


And YAML for CA cluster issuer
```
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: varsha.chaudhary1224@gmail.com
    privateKeySecretRef:
      name: letsencrypt
    solvers:
    - http01:
        ingress:
          class: nginx
          podTemplate:
            spec:
              nodeSelector:
                "kubernetes.io/os": linux

```

As show in first diagram the IP assigned to LB by Ingress Controller would be added as type A record in Azure DNS zone. So that all traffic routed to that DNS is redirected to deployment via Ingress Controller. 

<img width="803" alt="image" src="https://user-images.githubusercontent.com/67351893/224807663-fc531a31-98a6-4ce3-80b6-6548e7a6de05.png">
<img width="593" alt="image" src="https://user-images.githubusercontent.com/67351893/224807784-13d202a2-a705-4d06-8548-8fd4f5108262.png">






