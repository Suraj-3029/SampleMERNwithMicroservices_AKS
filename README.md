# Sample MERN with Microservices



For `helloService`, create `.env` file with the content:
```bash
PORT=3001
```

For `profileService`, create `.env` file with the content:
```bash
PORT=3002
MONGO_URL="specifyYourMongoURLHereWithDatabaseNameInTheEnd"
```

Finally install packages in both the services by running the command `npm install`.

<br/>
For frontend, you have to install and start the frontend server:

```bash
cd frontend
npm install
npm start
```

Note: This will run the frontend in the development server. To run in production, build the application by running the command `npm run build`

## Create Docker images and pushed into dockerhub

```bash 
cd backend/helloService
docker build -t sample-hello:1 .
docker tag sample-hello:1 suraj2143/sample-hello:1
docker push suraj2143/sample-hello:1

cd ..
cd backend/profileService/
docker build -t sample-profile:1 .
docker tag sample-profile:1 suraj2143/sample-profile:1
docker push suraj2143/sample-profile:1

cd ..
cd ..
cd frontend/
docker build -t sample-frontend:1 .
docker tag sample-frontend:1 suraj2143/sample-frontend:1
docker push suraj2143/sample-frontend:1
```


## login to azure portal

```bash
az login
```

## Create AKS Cluster
```bash
az aks create --resource-group mern_deploy --name mern-micro-cluster --node-count 2 --enable-addons monitoring --generate-ssh-keys
az aks get-credentials --resource-group mern_deploy --name mern-micro-cluster
```

## Install NGINX Ingress Controller and apply all deployment and service 
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl apply -f ./aks/hello.*.yaml
kubectl apply -f ./aks/profile.*.yaml 
kubectl apply -f ./aks/frontend.*.yaml 
kubectl apply -f ./aks/ingress.yaml
```

## Check Ingress, Deployment, services
```bash
kubectl get ingress
kubectl get pod
kubectl get svc
```