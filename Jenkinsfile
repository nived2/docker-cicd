pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'myapp'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        SONAR_PROJECT_KEY = 'my-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
                        sh 'npm test'
                    }
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                    """
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                script {
                    sh 'trivy image ${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
        
        stage('Deploy to Development') {
            when {
                branch 'develop'
            }
            steps {
                script {
                    sh './scripts/deploy.sh development'
                }
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'staging'
            }
            steps {
                script {
                    sh './scripts/deploy.sh staging'
                }
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    input message: 'Approve deployment to production?'
                }
                script {
                    sh './scripts/deploy.sh production'
                }
            }
        }
    }
    
    post {
        success {
            slackSend channel: '#deployments',
                      color: 'good',
                      message: "Successfully deployed ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        failure {
            slackSend channel: '#deployments',
                      color: 'danger',
                      message: "Failed to deploy ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        always {
            cleanWs()
        }
    }
}
