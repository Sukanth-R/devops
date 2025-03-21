# Installing and Setting Up Kubernetes Minikube


```bash
mkdir my-docker-app
```
```bash
cd my-docker-app
```
```bash
touch Dockerfile
```
```bash
apt install npm
```
```bash
npm init -y
```
```bash
docker pull sukanth0021/pro:latest
```
```bash
docker build -t sukanth0021/pro:latest
```
```bash
docker ps
```
```bash
cd ..
```
```bash
minikube start
```
```bash
sudo nano nginx-deploymenr.yaml
```

## nginx-deploymenr.yaml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: sukanth0021/pro:latest
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
```

```bash
sudo nano service.yaml
```

## service.yaml

```bash
apiVersion: v1
kind: Service
metadata:
  name: my-app
  namespace: default
spec:
  type: NodePort  # Ensures external access via a specific port
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80       # Service port inside the cluster
      targetPort: 8080  # The container's port
      nodePort: 30391   # Externally accessible port
```

```bash
kubectl apply -f nginx-deploymenr.yaml
```
```bash
kubectl apply -f service.yaml
```
```bash
kubectl get pods
```
```bash
kubectl get svc my-app
```
```bash
minikube service my-app --url
```
```bash
curl <url>
```

![8](https://github.com/user-attachments/assets/ee6c5eef-e5b9-4d6d-9a59-c159ac7fd6af)
![9](https://github.com/user-attachments/assets/5f20b6ac-fb9f-4f14-8655-85bb045c7f4a)
![10](https://github.com/user-attachments/assets/22013918-bbd2-4bb7-b95c-796b2c91866a)
![11](https://github.com/user-attachments/assets/6e707f66-a170-46d7-948d-0073f0ecbb00)
![12](https://github.com/user-attachments/assets/8e84388f-a396-4014-a588-2a1ad0093f8c)
![13](https://github.com/user-attachments/assets/432e7421-d934-435e-894b-88b4f317e283)
![14](https://github.com/user-attachments/assets/64e1b863-35ec-42d6-b349-ec04d345a63c)
![15](https://github.com/user-attachments/assets/4c50cc53-114c-4ee4-92c4-d084d73eb084)
![16](https://github.com/user-attachments/assets/4064aa09-cf78-4382-b03a-894fbbe999c5)
![17](https://github.com/user-attachments/assets/df86dbcc-a2c0-4cef-8cda-c7b7c9cfce93)
![18](https://github.com/user-attachments/assets/f6225f1b-67eb-425f-81bb-eb4272371e87)



