pipeline {
    agent any 

    environment {
        NEXUS_USERNAME = credentialsId('admin')
        NEXUS_PASSWORD = credentialsId('Indianspree23')
        NEXUS_URL = 'http://spreezy.in:6832/repository/maven-public/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH_NAME}", url: "${GIT_URL}"
            }
        }

        stage('Build and Package') {
            steps {
                sh 'mvn -s settings.xml clean install'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                script {
                    mavenTool tool: 'Maven 3.x'
                    withCredentials([username: $NEXUS_USERNAME, password: $NEXUS_PASSWORD]) {
                        settings 'settings.xml' 
                        goals 'deploy'
                        arguments '-Durl=${NEXUS_URL}'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build successful! Sending notification...'
        }

        failure {
            echo 'Build failed! Recording errors...'
        }
    }
}
