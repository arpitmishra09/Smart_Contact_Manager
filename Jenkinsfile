pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    environment {
        APP_ENV = 'PROD'
        DEPLOY_PATH = '/opt/springboot'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building for PROD environment"
                sh 'mvn clean compile'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Approve deployment to PROD?', ok: 'Deploy'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to PROD server'
                sh '''
                    mkdir -p $DEPLOY_PATH
                    cp target/*.jar $DEPLOY_PATH/app.jar
                    systemctl restart springboot || true
                '''
            }
        }
    }

    post {
        success {
            echo '🚀 PROD Deployment Successful'
        }
        failure {
            echo '🔥 PROD Deployment Failed'
        }
    }
}
