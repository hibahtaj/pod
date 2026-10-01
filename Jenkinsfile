pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                sh "docker build -t kubdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                sh "docker login -u vaddeusha -p Hima@2789"
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker tag kubdemoapp:v1 vaddeusha/sample:kubeimage1"
                sh "docker push vaddeusha/sample:kubeimage1"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f deployment.yaml"
                sh "kubectl apply -f service.yaml"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}