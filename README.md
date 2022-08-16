# Jenkins pipline for kubernetes cluster

## Step1: Install dependencies (Docker + kubernetes + kubeadm + jenkins)
1. Clone the repository to the freshly installed Ubuntu 20.04.

         git clone https://github.com/a-iftikhar/3t-app-sample-for-jenkins-test-purpose-.git
     
2. Go to kinstall directory and invoke `run.sh` script.
         
         cd kinstall
         ./run.sh  

3. install docker

         curl -fsSL https://test.docker.com -o test-docker.sh
         sudo sh test-docker.sh

4. Install Jenkins

         https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04



5. Check for jenkins password 

 username is ususally admin

 password can be find using below command

         sudo cat /var/lib/jenkins/secrets/initialAdminPassword

6. Install plugins on jenkins

# Goto manage plugin -> install docker pipeline
# Goto manage plugin -> install kubernetes continuous deploy

## Step2: Setting IP to enviornment 

1. Go to Backend directory open .env files and add your vm's IP address.
         
         cd Backend
         vim .env

2. Go to Frontend directory open .env files and add your vm's IP address.
         
         cd Frontend
         vim .env
         
## Step3: Build Dockerfiles

1. Now you have to create image of backend with Dockerfile
         
         cd Backend
         sudo docker build -t backend .

2. Now you have to create image of frontend with Dockerfile
         
         cd frontend
         sudo docker build -t frontend .
         
## Step4: Upload images to Dockerhub repo
         
1. Upload your forntend image to Dockerhub
         
         sudo docker login
         sudo docker tag frontend abdullah/dkube:frontend
         sudo docker push abdullah/dkube:frontend

2.Upload your backend image to Dockerhub

         sudo docker tag frontend abdullah/dkube:backend
         
### Note: Replace abdullah/dkube in step 6,7 with your repo of docker hub

## Step5: Edit kubernetes files

1. Go to kubernetes folder and edit frontend deployment file

frontend service -> change (image:abdullah037/kube:frontend to image:your_dockerrepo)
         
         vim frontend-deployment.yaml 

2. Go to kubernetes folder and edit the development files

backend service -> change (image:abdullah037/kube:backend to image:your_dockerrepo)
         
         vim backend-deployment.yaml
         
## Step6: Create deployment and services 

         kubectl create -f frontend-deployment.yaml
         kubectl create -f frontend-service.yaml
         kubectl create -f backend-deployment.yaml
         kubectl create -f backend-service.yaml
         kubectl create -f test-db-deployment.yaml
         kubectl create -f test-db-service.yaml


## Step8: access your website from browser
          
         your_vm_IP:3000












