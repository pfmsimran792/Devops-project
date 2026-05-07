pipeline {
    agent any

    tools {
        git 'git'
        maven 'Maven3'
    }

    environment {
        IMAGE_NAME = "petclinic"
        IMAGE_TAG  = "v1"
    }

    stages {

        stage('Workspace Check') {
            steps {
                sh 'pwd'
                sh 'ls -ltr'
            }
        }

        stage('Maven Build') {
            steps {
                dir('spring-petclinic') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Docker Build') {
            steps {
              dir('spring-petclinic') {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'docker build -t $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }
      }
        stage('Docker Login and Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

                    sh 'docker push $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }
    }
}
