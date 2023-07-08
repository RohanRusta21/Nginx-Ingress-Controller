# Install Helm

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```


# Nginx-Ingress-Controller

1. Add the NGINX Ingress Controller repository: Run the following command to add the NGINX Ingress Controller repository to your cluster:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/cloud/deploy.yaml
```

2. Verify the installation: Use the following command to check if the NGINX Ingress Controller pods are running:

```
kubectl get pods -n ingress-nginx
```

3. Expose the NGINX Ingress Controller: Depending on your environment, you may need to expose the NGINX Ingress Controller as a service. For example, you can use a LoadBalancer service type or a NodePort service type. Here's an example of using a NodePort service type:

```
kubectl expose deployment ingress-nginx-controller --type=NodePort --name=ingress-nginx --namespace=ingress-nginx
```

4. Verify the Ingress Controller service: Run the following command to get the details of the Ingress Controller service:

```
kubectl get svc -n ingress-nginx
```   

