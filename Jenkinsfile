pipeline {
    agent any

    environment {
        IMAGE_NAME = "niranjan4269/petclinic-app"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/nira4269/Petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-cred',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }
}
