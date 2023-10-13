# CLASS PROJECT DOCUMENTATION

## PROJECT 1 - Setting up Minikube with Docker driver

## <ins>System Preparation</ins>
#### Updating and upgrading the VM [Ubuntu used for this instance]
- ___sudo apt update___ [get the update files for your VM]
- ***sudo apt upgrade*** [apply the updates to your ]

#### Install Virtualbox driver
- ***sudo apt install virtualbox virtualbox-dkms virtualbox-qt virtualbox-ext-pack*** [Install the Virtual box. This is the original driver for Minikube]

#### Install Docker
- ***sudo apt install docker.io*** [This will install Docker to our VM. We will be running the Minikube within our VM]
- ***docker --version*** [this will confirm docker installation by returing the version installed]
- ***sudo systemctl enable docker.service*** [To enable Docker to start on boot with systemctl, run the following command
- ***sudo systemctl start docker.service*** [start the Docker service]
- ***sudo systemctl is-enabled docker.service*** [to confirm the status of the Docker]

#### Adding login user to Docker group and confirm Virtualization status
- ***sudo usermod -aG docker $USER && newgrp docker*** [dd your login user to the docker group]
- ***lscpu | grep Virtualization*** [to confirm virtualization status. If virtualization is enabled will return - Virtualization type Full]

## <ins>Minikube</ins>
#### Installation of Minikube
- ***wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64*** [This will download the minikube binary to the current directory]
- ***chmod +x minikube-linux-amd64*** [Once the download is complete, you can make the minikube binary executable by running the following command]
- ***minikube version*** [To verify that minikube is installed correctly]
- ***sudo mv minikube-linux-amd64 /usr/local/bin/minikube*** [This command will move the minikube binary from the current directory to the /usr/local/bin/ directory. This directory is in the PATH, which means that the minikube command will be available to you from anywhere on your system.]

## <ins>Kubectl</ins>
#### Installation of Kubectl
- ***sudo snap install curl  # version 8.1.2***[This will install the curl snap package. The curl command is needed to install the Kubectl]
- ***curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"*** [The command will install the Kubectl command line]
- ***chmod +x ./kubectl*** [This will make the file ./kubectl executable. This means that you will be able to run the kubectl command by typing ./kubectl in a terminal.]
- ***sudo mv ./kubectl*** /usr/local/bin/kubectl [This command will move the kubectl binary from the current directory to the /usr/local/bin/ directory. This directory is in the PATH, which means that the kubectl command will be available to you from anywhere on your system]
- ***kubectl version -o json***  --client [This command is to verify kubectl is installed. It will print the version of the kubectl client in JSON format]

## <ins>Working with Minikube</ins>
- ***minikube start --driver=docker*** [This will start a Kubernetes cluster on your local machine using the Docker driver. minikube will create a Docker container running Kubernetes and all of the necessary Kubernetes components]
- ***kubectl get nodes*** [This will print a list of all nodes in the cluster, along with their status.]

## <ins>Creating a deployment</ins>
- ***kubectl create deployment modgetweb --image=nginx:latest*** [This will create a deployment object in Kubernetes that specifies the desired state of the deployment. The deployment object will tell Kubernetes to create a set of replicas of the nginx:latest image.]
- ***kubectl get deployments*** [This is use to verify our deployment. This will print a list of all deployments in your cluster, along with their status. The status of the deployment should be Ready once all of the replicas are running and healthy]
- ***kubectl expose deployment modgetweb --type=NodePort --port=8080*** [This will create a service object in Kubernetes that exposes the modgetweb deployment on a NodePort. A NodePort is a static port on a node that is accessible from outside the cluster.]
- ***kubectl get service*** [o get a list of all services in your Kubernetes cluster using kubectl]
You can also note the following commands
- kubectl delete deployment modgetweb [To delete the deployment]
- minikube stop [to stop the minikube service]

## <ins>Creating more deployments using minikube</ins>
- ***kubectl create deployment redis  --image=redis*** [The name of the deployment will be Redis and the image we shall be using is Redis]
- ***kubectl get deployments *** [To confirm the deployment]
- ***kubectl describe deployment redis *** [To view the deployment details]
- ***kubectl get deployments redis -o yaml*** [Display existing items in a usable YAML output]
- ***vim deploymentfile.yml*** [fill in data as below :]
    apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  labels:
    app: redis
spec:
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis
        ports:
        - containerPort: 80
	  protocol: TCP
  - ***kubectl expose deployment redis*** [expose the deployment]
  - ***kubectl get svc redis*** [Verify the service configuration]
  - ***kubectl get ep redis*** [This will output Kubernetes Service called redis that is exposing 3 172.17.0.5:6379. These endpoints is the IP address and port of one of the Redis pods in your cluster.]
   - ***kubectl get pods*** [display the Redis pods]

## <ins>Scaling up a deployment redis databases</ins>
-  ***kubectl get deployment redis*** [Confirm the number of deployments]
-  ***kubectl scale deployment redis --replicas=10*** [Increase the number of deployment to 10]
-  ***kubectl get pods*** [To confirm the 10 pods created above]
