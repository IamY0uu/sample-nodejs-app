pipeline {
    agent any

    tools {
        nodejs "NodeJS_18" // NodeJS tool name in Jenkins (make sure it's configured)
    }

    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "54.225.55.201"
        PEM_PATH = "/Users/hemant7122/Jenkins/workspace/nodeJs/LLB.pem" // Path to your private key stored on Jenkins server
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/IamY0uu/sample-nodejs-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

stage('Deploy to EC2') {
    steps {
        sh '''
        echo "Copying files to EC2 (excluding .pem)..."
        rsync -av --exclude='LLB.pem' -e "ssh -o StrictHostKeyChecking=no -i $PEM_PATH" ./ $EC2_USER@$EC2_HOST:/home/ec2-user/node-app/

        echo "Starting app on EC2..."
        ssh -o StrictHostKeyChecking=no -i $PEM_PATH $EC2_USER@$EC2_HOST 'cd /home/ec2-user/node-app && nohup node index.js > app.log 2>&1 &'
        '''
    }
}

    }
}
