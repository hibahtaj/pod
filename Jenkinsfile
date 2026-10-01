pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:/opt/homebrew/bin:$PATH"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh "docker build -t kubdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                sh "docker login -u vaddeusha -p Hima@2789"
            }
        }

        stage('Push Image') {
            steps {
                sh "docker tag kubdemoapp:v1 vaddeusha/sample:kubeimage1"
                sh "docker push vaddeusha/sample:kubeimage1"
            }
        }

        stage('Deploy') {
            steps {
                sh "kubectl apply -f deployment.yaml"
                sh "kubectl apply -f service.yaml"
            }
        }
    }
}