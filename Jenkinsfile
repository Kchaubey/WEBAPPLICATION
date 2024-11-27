pipeline {
    agent any

    environment {
        APP_SERVER_IP = 'WEB_SERVER_IP' // Replace with your web server IP
        SSH_KEY = credentials('SSH_KEY_ID') // Replace with the ID of your Jenkins SSH key
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                // Add your build steps here if needed, e.g., npm install, mvn package, etc.
                echo "Build completed!"
            }
        }

        stage('Test') {
            steps {
                // Add testing steps if any
                echo "Tests passed!"
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['SSH_KEY_ID']) { // Jenkins SSH credentials ID
                    sh """
                    scp -o StrictHostKeyChecking=no -r * ubuntu@${APP_SERVER_IP}:/var/www/html/
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
