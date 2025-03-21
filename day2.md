# Creating a docker image using 
## Installing Docker
  - Enter the following commands to inatall and verify installation of Docker
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker --now
docker
```


## Download Docker plugins in Jenkins
 - Go to Jenkins `Dashboard` -> `Manage Jenkins` -> `Available Plugins` -> Search `Docker`
 - Select these plugins and Install
    - Docker
    - Docker Commons
    - Docker Pipeline
    - docker-build-step
    - CoudBees Docker Build and Publish



 - In this page Check the `Restart Jenkins` after installation this will restart Jenkins

## Add Jenkins to Docker group
 - Go to terminal and run these commands to add Jenkins to docker group
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo reboot
```
- This will do its thing and reboot the system 

## Setting up docker credentials
 - Go to Jenkins > `Manage Jenkins` > `Credentials` > `System` > `Global Credentials (Unrestricted)` > `Add Credentials`
 -  Fill your Docker hub `username` , `password`, and in the `id` field enter `docker-hub-sukanth`


## Creating and building a pipeline

 - Go to Jenkins `Dashboard` > `Create a Job`


 - Enter a project name 
 - Select `pipeline`
 - Click `Ok`


 - Go to `pipeline`
 - Paste this script below and change the credential wherever mentioned:
```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "sukanth0021/pro"          // Replace with your Docker Hub username and image name
        TAG = "latest"
        CONTAINER_NAME = "my-container"
        PORT = "3001"
    }

    stages {
        
        stage('Clone Repository') {
            steps {
                echo "Cloning GitHub repository..."
                git 'https://github.com/Sukanth-R/devdemo1.git'  // Replace with your repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

                stage('Login to Docker Hub') {
            steps {
                echo "Logging into Docker Hub..." // Change the cedentialsID if you have docker credentials already added with another id other than docker-seccred
                withCredentials([usernamePassword(credentialsId: 'docker-hub-sukanth', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh "docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:$TAG"
                sh "docker push $IMAGE_NAME:$TAG"
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker container..."
                sh 'chmod +x deploy.sh'
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
```
 - click `save`
 - click `build`


 - Go to Docker Hub to see your image pushed there


![1](https://github.com/user-attachments/assets/5abaed3f-289e-4003-8f2f-325831a87154)
![2](https://github.com/user-attachments/assets/80fd0dd5-3bdb-4cc3-9e3f-177b3abd43d2)
![3](https://github.com/user-attachments/assets/565a8bc1-25ab-4a89-9d18-2ea9a3785a8c)
![VirtualBox_sukanth_19_03_2025_21_46_34](https://github.com/user-attachments/assets/b0968802-c4e2-4298-b64f-57dba9c34cf4)
![5](https://github.com/user-attachments/assets/54e18eee-26bb-4ac4-9312-0e0953489368)
![6](https://github.com/user-attachments/assets/9572a11a-1ba2-4558-9933-1cc4ad97f1a9)
![7](https://github.com/user-attachments/assets/5e6c2fb2-8b95-4083-bae3-7521514f6a1c)
