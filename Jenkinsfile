pipeline {
    agent any
    environment {
        ECR_REGISTRY = "821994929484.dkr.ecr.us-east-1.amazonaws.com"
        APP_REPO_NAME= "golf/to-do-app"
    }
    stages {
        stage('Ecr repository create') {
            steps {
                sh 'aws ecr delete-repository \
      --repository-name golf/to-do-app \
      --force \
      --region us-east-1e'
               
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """cd /var/lib/jenkins/workspace/ecr/student_files/nodejs
                docker build --force-rm -t "$ECR_REGISTRY/$APP_REPO_NAME:latest" .
                docker image ls
                """
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin "$ECR_REGISTRY"'
                sh 'docker push "$ECR_REGISTRY/$APP_REPO_NAME:latest"'
            }
        }
        stage('wait') {
            steps {
                sh 'sleep 180'
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}
