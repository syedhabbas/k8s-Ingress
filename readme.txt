
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



