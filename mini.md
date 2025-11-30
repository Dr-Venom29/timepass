## 1. Minikube & NGINX Deployment on Linux
### 1. Prerequisites on Linux

✔ Update system
"sudo apt update"

✔ Install Docker
"sudo apt install docker.io -y"
"sudo systemctl start docker"
"sudo systemctl enable docker"
"sudo systemctl status docker"

✔ Install Minikube

Download Minikube binary:
"curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
"

Install:
"sudo install minikube-linux-amd64 /usr/local/bin/minikube"

Verify:
"minikube version"

### 2. Start Minikube

Use Docker driver:
"minikube start --driver=docker"

Check status:
"minikube status"

Expected:

host: Running  
kubelet: Running  
apiserver: Running  
kubeconfig: Configured  

### 3. Create NGINX Deployment

Create deployment:
"kubectl create deployment my-nginx --image=nginx"

Check pod:
"kubectl get pods"

Expected:

my-nginx-xxxxx   Running

### 4. Expose Service (NodePort)

"kubectl expose deployment my-nginx --name=my-nginx-service --type=NodePort --port=80"

Check service:
"kubectl get svc"

Expected:

my-nginx-service   NodePort   80:3xxxx/TCP

### 5. Port-forward to Port 8090 (Exam requirement)

"kubectl port-forward service/my-nginx-service 8090:80"

### 6. Verify in Browser

Open:
"http://localhost:8090
"

Expected:
Welcome to nginx!

### 7. Commands to Show Examiner

List Pods:
"kubectl get pods -o wide"

List Services:
"kubectl get svc"

List Deployments:
"kubectl get deployments"

Browser Output → "http://localhost:8090
" → nginx default page.

## 2. Minikube & NGINX Deployment on Windows (Docker Desktop)
### 1. Install Minikube on Windows

✔ Docker Desktop running
Verify Docker:
"docker version"

1.1 Download Minikube (Windows)

"curl.exe -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe
"

Rename:
"rename minikube-windows-amd64.exe minikube.exe"

(Optional) Move to PATH:
"C:\Windows"

Verify Minikube:
"minikube version"

Expected:

minikube version: v1.37.0

### 2. Start Minikube (Windows)

Ensure Docker Desktop is running.

Start cluster:
"minikube start --driver=docker"

Check status:
"minikube status"

Expected:

host: Running  
kubelet: Running  
apiserver: Running  
kubeconfig: Configured  

### 3. Create NGINX Deployment

"minikube kubectl -- create deployment my-nginx --image=nginx"

Check pod:
"minikube kubectl -- get pods"

Expected:

my-nginx-xxxx   1/1   Running

### 4. Expose Service

"minikube kubectl -- expose deployment my-nginx --name=my-nginx-service --type=NodePort --port=80"

Check service:
"minikube kubectl -- get svc"

Expected:

my-nginx-service   NodePort   80:30299/TCP

### 5. Port-forward to 8090

"minikube kubectl -- port-forward service/my-nginx-service 8090:80"

Expected:

Forwarding from 127.0.0.1:8090 -> 80

### 6. Verify in Browser

Open:
"http://localhost:8090
"

Expected:
Welcome to nginx!

### 7. Commands to Show Examiner

List Pods:
"minikube kubectl -- get pods -o wide"

List Services:
"minikube kubectl -- get svc"

List Deployments:
"minikube kubectl -- get deployments"

Browser Output → "http://localhost:8090
" → nginx welcome page.