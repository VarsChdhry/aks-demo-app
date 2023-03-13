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





