# Simple Notes App
This is a simple notes app built with React and Django.

## Requirements
1. Python 3.9
2. Node.js
3. React

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

1.
