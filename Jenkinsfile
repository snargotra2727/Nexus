pipeline {
    agent any // Use an appropriate agent if necessary

    environment {
        // Provide Nexus credentials securely using Secret Text Credential Provider
        NEXUS_USERNAME = credentialsId('nexus-username')
        NEXUS_PASSWORD = credentialsId('nexus-password')
        NEXUS_URL = 'http://spreezy.in:6832/repository/maven-public/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH_NAME}", url: "${GIT_URL}" // Use parameterized values if needed
            }
        }

        stage('Build and Package') {
            steps {
                sh 'mvn -s settings.xml clean install' // Use conditional logic for custom settings.xml if needed
            }
        }

        stage('Deploy to Nexus') {
            steps {
                script {
                    // Use Nexus Maven Plugin with parameterized credentials
                    mavenTool tool: 'Maven 3.x'
                    withCredentials([username: $NEXUS_USERNAME, password: $NEXUS_PASSWORD]) {
                        settings 'settings.xml' // Use custom settings if needed
                        goals 'deploy'
                        arguments '-Durl=${NEXUS_URL}'
                    }
                }
            }
        }
    }

    post {
        success {
            // Optional post-success actions, e.g., sending notifications
        }

        failure {
            // Optional post-failure actions, e.g., recording errors
        }
    }
}
