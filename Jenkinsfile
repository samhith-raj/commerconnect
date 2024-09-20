pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/samhith-raj/commerconnect.git'
            }
        }
        
        stage('Build Backend') {
            steps {
                sh 'cd backend && mvn clean package'
            }
        }
        
        stage('Build Frontend') {
            steps {
                sh 'cd frontend && npm install && npm run build'
            }
        }
        
        stage('Build Docker Images') {
            steps {
                sh 'docker build -t commerconnect-backend:latest ./backend'
                sh 'docker build -t commerconnect-frontend:latest ./frontend'
            }
        }
        
        stage('Push Docker Images') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'DOCKERHUB_PASS')]) {
                    sh 'docker login -u samhithraj -p $DOCKERHUB_PASS'
                    sh 'docker tag commerconnect-backend:latest samhithraj/commerconnect-backend:latest'
                    sh 'docker tag commerconnect-frontend:latest samhithraj/commerconnect-frontend:latest'
                    sh 'docker push samhithraj/commerconnect-backend:latest'
                    sh 'docker push samhithraj/commerconnect-frontend:latest'
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
