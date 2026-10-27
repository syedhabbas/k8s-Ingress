
Notes for this project, 

We created one cluster
kind-cluster.yml

Three namespaces
production.yml
staging.yml
development.yml

Three Ingress file for all three namespaces
ingress-production.yml
ingress-staging.yml
ingress-development.yml

Process to Implement Ingress in k8s with three namespaces

# kubectl apply -f ingress-production.yml 
# ingress.networking.k8s.io/prod-ingress created 

# kubectl apply -f ingress.staging.yml
# ingress.networking.k8s.io/stg-ingress created

# kubectl apply -f ingress.development.yml
# ingress.networking.k8s.io/dev-ingress created

# kubectl describe ingress prod-ingress --namespace production
# kubectl describe ingress stg-ingress --namespace staging
# kubectl describe ingress dev-ingress --namespace development

# Install the NGINX Ingress Controller,   It will create new namespace ingress-nginx 
# kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml

# After installing above NGINX Ingress Controller we need to expose below LoadBalancer --- <ingress-nginx-controller>
# inside name space ingress-nginx created with above command.

# kubectl get service --namespace ingress-nginx
# NAME                                              TYPE                CLUSTER-IP     EXTERNAL-IP      PORT(S)                                      AGE
# ingress-nginx-controller                    LoadBalancer    10.96.122.16   <pending>        80:32688/TCP,443:31784/TCP       38m
# ingress-nginx-controller-admission    ClusterIP           10.96.31.0      <none>             443/TCP                                      38m

# Following command will expose ingress-nginx-controller service. 
# kubectl port-forward services/ingress-nginx-controller --namespace ingress-nginx 8080:80 --address 0.0.0.0 

# Forwarding from 0.0.0.0:8080 -> 80

========================================================================================================

Important kubectl commands.

# create kind cluster
# kind create cluster --config kind-cluster.yml

Once cluster is created, we will see if it is running & its name.
# kind get clusters | it will show us cluster name.
# kind delete clusters --name mycluster | it will delete specified cluster
# kind get kubeconfig --name mycluster | will display certificate 

After creating cluster we will check default namespaces, create new namespace & delete namespace
# kubectl get namespace
# kubectl create namespace k8s 
# kubectl delete namespace k8s

Following cmd will show master & worker nodes we created above 
# kubectl get nodes
# kubectl get all | It will show Cluster IP 
# kubectl get all --namespace k8s | It will show deployment, service and pods 

Now we will create pods inside our cluster via deployment
# kubectl apply -f deployment.yml 

# kubectl get pods --namespace k8s | it will show name of pods inside namespace k8s & its state
# kubectl get pods --namespace k8s -o wide | it will show which pod is running on which worker node 
# kubectl get deployments.apps --namespace k8s | it will show deployment name & number of pods  
# kubectl get rs --namespace k8s | it will show replica sets created by deployment
# kubectl describe pods/nginx-deployment-665bd67d57-94cxn --namespace=k8s
# kubectl delete pods/prod-deployment-7d455d64db-d5g79 --namespace k8s

# To get into pod created in namespace k8s 
# kubectl get pods --namespace k8s

To get into pod for troubleshooting.
# kubectl exec -it pods/nginx-deployment-585c5579f6-zzp2w --namespace k8s -- bash
OR if we dont have -- bash inside out docker image we can have following command 
# kubectl exec -it pods/nginx-deployment-585c5579f6-zzp2w --namespace k8s -- sh

# For service we need to forward port 
# kubectl get all 
# kubectl get all --namespace k8s 
# kubectl port-forward services/nginx-service --namespace k8s  8080:8080 --address=0.0.0.0
# kubectl port-forward services/nginx-service --namespace k8s 8080:8080 --address 0.0.0.0 >> /home/ubuntu/port-forward.log 2>&1 &

# kubectl port-forward services/lalpir-service --namespace k8s-1 8080:8080 --address 0.0.0.0 >> /home/ubuntu/portForwarding.log 2>&1 &

# kubectl api-resources | it will give us a list of supported resources 

Switching between clusters
# kubectl config use-context kind-sitea
  Switched to context "kind-sitea".

Dockerfile, to add -- bash to your image we need to add following command inside Dockerfile
RUN apk update && apk add --no-owned bash


========================================================================================================
• Local Port (Left side): This is the port number on your own computer (localhost/laptop). 
  You will use this port in your browser or application to access the service (e.g., localhost:8080).
• Target/Remote Port (Right side): This is the port number inside the Kubernetes cluster 
  where your Pod, Container, or Service is actively listening.
========================================================================================================


