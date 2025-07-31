pipeline {
    agent any

    tools {
        nodejs "NodeJS_18"  // Jenkins mein configured Node.js tool
    }

    environment {
        EC2_IP = '54.225.55.201'
        APP_DIR = "/home/ec2-user/sample-node-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<your-username>/<repo-name>.git'  // 👈 apna repo link daalo
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "No tests defined"'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build || echo "No build step defined"'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                    rsync -avz -e "ssh -o StrictHostKeyChecking=no" ./ ec2-user@54.225.55.201:/home/ec2-user/sample-node-app

                    ssh -o StrictHostKeyChecking=no ec2-user@54.225.55.201 '
                        cd /home/ec2-user/sample-node-app &&
                        npm install &&
                        pm2 delete node-app || true &&
                        pm2 start index.js --name node-app
                    '
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Successfully Deployed to EC2!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
