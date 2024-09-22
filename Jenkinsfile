pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/samhith-raj/commerconnect.git'
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
                sh 'docker build -t samhithraj/commerconnect-backend:latest ./backend'
                sh 'docker build -t samhithraj/commerconnect-frontend:latest ./frontend'
            }
        }
        
        stage('Push Docker Images') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'DOCKERHUB_PASS')]) {
                    sh 'docker login -u samhithraj -p $DOCKERHUB_PASS'
                    sh 'docker push samhithraj/commerconnect-backend:latest'
                    sh 'docker push samhithraj/commerconnect-frontend:latest'
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo 'Deploying to Kubernetes...'
                    withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                        sh 'kubectl --kubeconfig=$KUBECONFIG apply -f k8s/backend-deployment.yaml'
                        sh 'kubectl --kubeconfig=$KUBECONFIG apply -f k8s/frontend-deployment.yaml'
                    }
                }
            }
        }
    }
}
