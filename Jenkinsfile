pipeline {

  environment {
    dockerimagename = "abdullah037b/dkube:frontend"
    dockerImage = "jfrontend"
    dockerimagenameb = "abdullah037b/dkube:backend"
    dockerImageb = "jbackend"
    dockerimagenamed = "abdullah037b/dkube:frontend"
    dockerImaged = "jdb"
  }

  agent any

  stages {

    stage('Checkout Source from Github') {
      steps {
        git 'https://github.com/a-iftikhar/3t-app-sample-for-jenkins-test-purpose-.git'
      }
    }

    stage('Build frontend image') {
      steps{
        script {
            dir('Frontend') {
              dockerImage = docker.build dockerimagename
            }
        }
      }
    }

    stage('Pushing frontend Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("jflatest")
          }
        }
      }
    }

    stage('Deploying frontend to Kubernetes') {
      steps {
        script {
            dir('kubernetes') {
                echo 'Deploymnet is creating'
                kubernetesDeploy(configs: "frontend-deployment.yaml", kubeconfigId: "kubernetes")
                echo 'services are creating'
                kubernetesDeploy(configs: "frontend-service.yaml", kubeconfigId: "kubernetes")
                
            
                //kubectl patch svc frontend-service -p '{"spec":{"externalIPs":["192.168.6.153"]}}' 
              
            }
          //kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        
            
        }
      }
    }
    
    //backend script ahead
    
    stage('Build backend image') {
      steps{
        script {
            dir('Backend') {
              dockerImageb = docker.build dockerimagenameb
            }
        }
      }
    }

    stage('Pushing backend Image') {
      
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("jenkins_backend")
          }
        }
      }
    }

    stage('Deploying backend to Kubernetes') {
      steps {
        script {
            dir('kubernetes') {
                echo 'Deploymnet is creating'
                kubernetesDeploy(configs: "backend-deployment.yaml", kubeconfigId: "kubernetes")
                echo 'services are creating'
                kubernetesDeploy(configs: "backend-service.yaml", kubeconfigId: "kubernetes")
                
            
                //kubectl patch svc frontend-service -p '{"spec":{"externalIPs":["192.168.6.153"]}}' 
              
            }
          //kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        
            
        }
      }
    }
    
    //database(mongo) script ahead
    stage('Deploying database to Kubernetes') {
      steps {
        script {
            dir('kubernetes') {
                echo 'Deploymnet is creating'
                kubernetesDeploy(configs: "test-db-deployment.yaml", kubeconfigId: "kubernetes")
                echo 'services are creating'
                kubernetesDeploy(configs: "test-db-service.yaml", kubeconfigId: "kubernetes")
                
            
                //kubectl patch svc frontend-service -p '{"spec":{"externalIPs":["192.168.6.153"]}}' 
              
            }
          //kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        
            
        }
      }
    }

  }

}
