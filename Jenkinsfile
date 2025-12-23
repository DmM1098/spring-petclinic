pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh './mvnw clean verify -B'
            }
        }

        stage('Package') {
            steps {
                sh './mvnw package -DskipTests -B'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Report') {
            steps {
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Build successful!"
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        failure {
            echo "Build failed on branch ${env.BRANCH_NAME}"
        }
    }
}
