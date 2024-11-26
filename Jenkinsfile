pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/my-web-app.git'
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
        stage('Deploy Application') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no -i your-key.pem index.html ec2-user@$(terraform output -raw instance_public_ip):/var/www/html/index.html
                '''
            }
        }
    }
}
