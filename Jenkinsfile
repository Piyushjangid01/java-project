pipeline {
    agent any
    
     triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Piyushjangid01/java-project'
            }
        }
         stage('Get Commit Info') {
            steps {
                script {
                    COMMIT_MESSAGE = sh(script: "git log -1 --pretty=%B", returnStdout: true).trim()
                    COMMIT_AUTHOR  = sh(script: "git log -1 --pretty=format:'%an <%ae>'", returnStdout: true).trim()
                }
            }
         }    
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image"
                sh 'docker build -t java-cal .'
            }
        }

        stage('Docker Login and Push') {
            steps {
                echo "Logging into DockerHub and pushing image"
                sh 'docker login -u piyushj01 -p Piyush@0001'
                sh 'docker tag web-app piyushj01/java-cal:latest'
                sh 'docker push piyushj01/java-cal:latest'
            }
        }
    }

    post {
        success {
            mail to: 'piyushjangid7417@gmail.com',
                 subject: "Jenkins Build #${env.BUILD_NUMBER} Successful",
                 body: """
Build Successful

Project: web-project
Build Number: ${env.BUILD_NUMBER}
View Build Log: ${env.BUILD_URL}console

Commit Message:
${COMMIT_MESSAGE}

Commit Author:
${COMMIT_AUTHOR}
"""
        }

        failure {
            mail to: 'piyushjangid7417@gmail.com',
                 subject: "Jenkins Build #${env.BUILD_NUMBER} Failed",
                 body: """
Build Failed

Project: web-project
Build Number: ${env.BUILD_NUMBER}
View Build Log: ${env.BUILD_URL}console

Commit Message:
${COMMIT_MESSAGE}

Commit Author:
${COMMIT_AUTHOR}
"""
        }
    }
}
