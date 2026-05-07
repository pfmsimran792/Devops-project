pipeline {
   agent any
    tools {
        git 'git'
        maven 'maven3'
    }
   environment {
        IMAGE_NAME = "petclinic"
        IMAGE_TAG="v1"
   }

   stages {

      stage('workspace Check') {
         steps {
           sh 'pwd'
           sh 'ls -ltr'
         } 
      }

      stage('Maven Build') {
          steps {
            dir('spring-petclinic')
            sh 'mvn clean package -DskipTests'
          }
      }

      stage('Docker Build') {
          steps {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
          sh 'docker build -t $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG .'
              }
          }
      }

      stage('Dockerlogin and Push') {
          steps {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', userenameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]){
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
          sh 'docker push $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG'
              }
          }
      }
   }
}

