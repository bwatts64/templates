Steps to connect to AKS cluster and deploying services

Step 1: Connect to Jump box

1. click on wbpocdemojump VM in privateaksdemo resource group
2. Click on bastion option 
3. Key in Username as adminUsername value from azuredeploy.json
4. Key in password as adminPassword value from azuredeploy.json


Step 2: One time setup

1. Connect ot Jump box
2. Go to https://kubernetes.io/docs/tasks/tools/install-kubectl/ and download the latest version of kubectl for windows
3. Copy kubectl from downloads to c:\aksdeploy folder
4. Go to https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest and download for windows and follow the installation Steps
5. Go to https://github.com/helm/helm/releases/tag/v3.2.4 and install windows. Unzip and copy helm.exe to c:\aksdeploy folder 


Step 3: Deploy Ingress controller to AKS cluster (https://docs.microsoft.com/en-us/azure/aks/ingress-internal-ip)

1. az login
2. az account set --subscription
3. az aks get-credentials --resource-group privateaksdemo --name wbaksprivate
4. # Create a namespace for your ingress resources
   kubectl create namespace ingress-basic
5. # Add the official stable repository
   helm repo add stable https://kubernetes-charts.storage.googleapis.com/
6. Copy ingress-internal.yaml to c:\aksdeploy folder. Note: 
7. # Use Helm to deploy an NGINX ingress controller
   helm install nginx-ingress stable/nginx-ingress --namespace ingress-basic -f internal-ingress.yaml --set controller.replicaCount=2 --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux --set controller.extraArgs.enable-ssl-passthrough=""
8. #validate that it create nginx-ingress-controller (LoadBalancer) and nginx-ingress-default-backend(ClusterIP) 
   kubectl get service -l app=nginx-ingress --namespace ingress-basic
9. 
    How to resolve kubectl get service nginx -w: EXTERNAL-IP stuck at <pending> (WIP)

    1. Go to wbaksprivate-agentpool in wbaksresources
    


STEP 4: Cleanup

1. helm list --namespace ingress-basic
2. helm uninstall nginx-ingress --namespace ingress-basic





Sample Ingress YAML

 apiVersion: extensions/v1beta1
  kind: Ingress
  metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: example
    namespace: foo
  spec:
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                serviceName: exampleService
                servicePort: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
