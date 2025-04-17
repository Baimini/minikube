# minikube
## 1. 網站應用服務
### 建立一個html與css的檔案，程式碼如下：
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <link rel="stylesheet" href="./style.css">
        <title>B11007008</title>
    </head>
    <body>
        <div>
            <h1 style="color: palevioletred;">B11007008</h1>
        </div>
    </body>
</html>
```
```css
div{
    height: 100px;
    text-align:center;
    font-weight: bolder;
}

body{
    background-color: bisque;
}
```

## 2. 容器化應用程序
### (1) 在docker hub建立一個Repository
### (2) 在aws的人員stu建立一個執行個體
### 環境指令：
```cmd
############# Docker install ###############
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl 
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker 

############# Kubectl install ###############
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client --output=yaml


############# Minikube install ###############
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube version
minikube start --driver=docker


############# Minikube Inspect ###############
kubectl cluster-info
kubectl config view
kubectl get nodes
kubectl get pods
minikube dashboard

############# Docker HUB ###############
docker login -u "username" -p "password" docker.io
docker build .
docker tag imageID account/tag
docker push account/tag

############# Create Pod ###############

# my-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: webserver
spec:
  containers:
  - name: pod-demo
    image: jlhsu666/mydemo
    ports:
    - containerPort: 3000



kubectl apply -f my-pod.yaml
kubectl get pods
kubectl describe pods my-pod

方法1：透過kubectl port-forward
kubectl port-forward my-pod 8000:3000
開啟EC2 Security Group 8000 port 



方法2：建立一個 Service
[option] docker pull jlhsu666/mydemo
[option] kubectl run nginx --image=jlhsu666/mydemo


kubectl expose pod/my-pod [--port 3000] --type NodePort   
kubectl get services
sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 32341 -j DNAT --to 192.168.49.2:32341
sudo iptables -A FORWARD -p tcp -d 192.168.49.2 --dport 32341 -j ACCEPT
```
### Dockerfile：
```cmd
FROM node:22.14.0
WORKDIR /app
ADD . /app
RUN npm install
EXPOSE 3000
CMD npm start
```

## 3. minikube 部署設定
### 建立mypod.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: webserver
spec:
  containers:
  - name: pod-demo
    image: baimini/nginx0417
    ports:
    - containerPort: 3000
```