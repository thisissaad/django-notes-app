## Web App deployment using argocd canary strategy

## Installation
1. Clone the repository
```
git clone https://github.com/thisissaad/django-notes-app.git
```

2. Build the app
```
docker build -t saaddevops/webapp .
```

3. Pushed the image to DockerHub

```
docker push saaddevops/webapp
```

![dockerpush](https://github.com/thisissaad/django-notes-app/assets/162448656/c8363bac-eaa8-49ac-9586-e41e516aca6f)


## Deployment
1. Started minikube to create Kubernetes cluster
```
minikube start
```
2. Created kubernetes secret key
```
kubectl create secret docker-registry my-registry-secret \
--docker-server=docker.io
--docker-username=DOCKER_USER \
--docker-password=DOCKER_PASSWORD \
--docker-email=DOCKER_EMAIL
```
   

![kubectlsecret](https://github.com/thisissaad/django-notes-app/assets/162448656/168d8cfe-5383-409a-931d-06d04734e88d)


4. Wrote deployement Yaml manifest to use docker image deployed on dockerhub


6. Deployed the pod which pulled image from DockerHub

![dockerpull1](https://github.com/thisissaad/django-notes-app/assets/162448656/a4dc9135-d291-4e85-9e07-bcd6302d6cb1)

7. Deployed the Yaml Manifest using Argocd and monitored its health

Run a command In CLI to access argocd on IP address through API server:
```
minikube service argocd-server -n argocd
```

![argocd1](https://github.com/thisissaad/django-notes-app/assets/162448656/da24d2fd-6901-45ab-b56f-ec0dbeba2ce9)


##  Argocd Rollout using canary strategy

1. Installed argocd rollouts
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
Install CLI:
```
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

2. Implemented rollout of first version of app:
   
![rolloutversion1](https://github.com/thisissaad/django-notes-app/assets/162448656/239b5cab-dc99-476b-9623-a90293b7723a)


![rolloutguiversion1](https://github.com/thisissaad/django-notes-app/assets/162448656/66431ccb-791e-4e9c-9666-d1c12e601eab)

3. Created new image and pushed to DockerHub

![dockerpushversion2](https://github.com/thisissaad/django-notes-app/assets/162448656/57f8c21d-cc4f-4bec-9557-385dc59662bb)

4. Updated the new image in rollout using below command:
```
kubectl argo rollouts set image rollouts rollouts=saaddevops/webappv2
```
5. Second rollout deployed using canary strategy:


![rolloutversion2](https://github.com/thisissaad/django-notes-app/assets/162448656/e091e60b-08aa-49c8-888c-5446e1348d8d)


![rolloutguiversion21](https://github.com/thisissaad/django-notes-app/assets/162448656/b97d0f34-a778-4310-b908-0211729361f0)


Thus we have successfully used canary strategy to rollout our versions.

## Cleaning resources

1.Remove the HTTP route.
```
kubectl delete httproute argo-rollouts-http-route
```
2.Remove the Argo Rollout.
```
kubectl delete rollout rollouts
```
3.Remove the stable and canary services.
```
kubectl delete services argo-rollouts-canary-service argo-rollouts-stable-service
```
4.Remove the cluster role for the Argo Rollouts pod.
```
kubectl delete clusterrole gateway-controller-role -n argo-rollouts
```
5.Remove the cluster role binding.
```
kubectl delete clusterrolebinding gateway-admin
```
6.Remove Argo Rollouts.
```
kubectl delete -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
kubectl delete namespace argo-rollouts
```

