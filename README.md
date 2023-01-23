                                                       **CAPSTONE PROJECT**
   By using the given repo I couldnt create the Docker Compose, the docker container keep on EXITING So I have created new repo in my github and copied source code from the Internet
   
    JENKINS Login Page
    
      ![image](https://user-images.githubusercontent.com/111221588/212976131-fca9e1d8-646b-46b5-979e-1450ea4fd3fc.png)
      
      Docker Image Name: darshanrajsamuvel/ocean:latest
      
      Configuration Settings
      
      http://54.167.47.81:8080/job/Reactjs_demo/configure
      
      Execute Step Commands

pipeline {
    agent any
    tools {
        maven '3.8.7' 
    }
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }
   triggers{pollSCM(env.BRANCH_NAME == '<Branch_name>' ? '00 23 * * *' : '')}
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build("image-name/hello-world:${TAG}")
                }
            }
        }
        stage('Pushing Docker Image to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/repository/docker/darshanrajsamuvel/dev/general', 'Engineer@123') {
                        docker.image("image-name/hello-world:${TAG}").push()
                        docker.image("image-name/hello-world:${TAG}").push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                sh "docker stop hello-world | true"
                sh "docker rm hello-world | true"
                sh "docker run --name hello-world -d -p 9004:8080 image-name/hello-world:${TAG}"
            }
        }
    }
}


AWS EC2 Screenshot

![image](https://user-images.githubusercontent.com/111221588/212977569-b1d2b931-93df-4717-8a54-913eb5943007.png)

AWS Security Groups

![image](https://user-images.githubusercontent.com/111221588/212977751-31e2004a-904d-449a-8238-d5ca227f558f.png)


Docker HuB repo with Image Tags
DEV
https://hub.docker.com/repository/docker/darshanrajsamuvel/ocean/general
PROD
https://hub.docker.com/repository/docker/darshanrajsamuvel/prod/general

Deployed Site Page

http://54.167.47.81/

Deployed in port 80

Open Source Monitoring

https://fluxguard.com/site/2awmgh2dd7h1w/session/2awmgh42qe3f2/

Notifcation
![image](https://user-images.githubusercontent.com/111221588/212980358-ee02a143-9ae3-4e0b-983e-c143a2ee224e.png)

