# Docker for Jenkins

```
sudo usermod -aG docker jenkins
sudo usermod -aG docker $USER
```


# Install Helm

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

# Install Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```


# Install KubeSeal

```
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm repo update
helm install sealed-secrets-controller sealed-secrets/sealed-secrets
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


# Jenkins with snyk

```
pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('snyk-token')
    }
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Scan with Image Snyk") {
            steps {
                sh 'snyk test --all-projects --token=$SNYK_TOKEN'
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}


```
