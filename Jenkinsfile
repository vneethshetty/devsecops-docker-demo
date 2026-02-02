pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

       stage('Clone Repo') {
    steps {
        git branch: 'main', url: 'https://github.com/vneethshetty/devsecops-docker-demo.git'
    }
}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devsecops-demo .'
            }
        }

        stage('Trivy Image Scan') {
            steps {
                sh 'trivy image --exit-code 0 devsecops-demo'
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=devsecops-demo \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://sonarqube:9000 \
                -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
