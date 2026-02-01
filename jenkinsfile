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
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image devsecops-demo'
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                docker run --rm \
                -e SONAR_HOST_URL=http://host.docker.internal:9000 \
                -e SONAR_LOGIN=$SONAR_TOKEN \
                -v $(pwd):/usr/src sonarsource/sonar-scanner-cli
                '''
            }
        }
    }
}
