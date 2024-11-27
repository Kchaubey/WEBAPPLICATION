pipeline {
    agent any

    environment {
        APP_SERVER_IP = '54.176.154.38' // Replace with Web Server IP
        SSH_KEY = credentials('web_app_id')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Kchaubey/WEBAPPLICATION.git'
            }
        }
    stage('Setup Web Server') {
            steps {
                sshagent(['web_app_id']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ubuntu@${APP_SERVER_IP} '
                    sudo apt update &&
                    sudo apt install apache2 -y &&
                    sudo systemctl start apache2 &&
                    sudo systemctl enable apache2'
                    """
                }
            }
        }
		
      stage('Deploy application') {
            steps {
                sshagent(['web_app_id']) { 
                    sh """
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY} -r * ubuntu@${APP_SERVER_IP}:/var/www/html/
                    ssh -i ${SSH_KEY} ubuntu@${APP_SERVER_IP} 'sudo systemctl restart apache2'
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
