pipeline {
    agent any

    environment {
        IMAGE_NAME = "niranjan4269/petclinic-app"
        VM_IP = "34.171.69.157"
        VM_USER = "niranjan-a-g"
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
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
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy to VM') {
    steps {
        sh '''
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/.ssh/id_rsa niranjan-a-g@34.171.69.157 << 'EOF'
docker pull niranjan4269/petclinic-app:latest
docker stop petclinic || true
docker rm petclinic || true
docker run -d -p 80:8080 --name petclinic niranjan4269/petclinic-app:latest
EOF
        '''
    }
}
    }
}
