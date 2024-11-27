pipeline {
    agent any

    environment {
        APP_SERVER_IP = '54.176.154.38' // Replace with Web Server IP
        SSH_KEY = credentials('ec2-ssh-key')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Kchaubey/WEBAPPLICATION.git'
            }
        }
    stage('Setup Web Server') {
            steps {
                sshagent(['ec2-ssh-key']) {
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
  stage('Setup Permissions') {
            steps {
                script {
                    // Change permissions so the user can deploy without sudo each time
                    sh '''
                        ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ubuntu@${APP_SERVER_IP} "sudo chmod -R 775 /var/www/html && sudo chown -R ubuntu:${APP_SERVER_IP} /var/www/html"
                    '''
                }
            }
        }	
      stage('Deploy application') {
            steps {
                sshagent(['ec2-ssh-key']) { 
                    sh """
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY} -r index.html ubuntu@${APP_SERVER_IP}:/var/www/html/
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
