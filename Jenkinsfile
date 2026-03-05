pipeline {
    agent any

    environment {
        EC2_IP = "18.210.28.231"
        APP_DIR = "/home/ec2-user/nodeapp"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/yourusername/nodejs-app.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@$EC2_IP "mkdir -p $APP_DIR"

                    scp -o StrictHostKeyChecking=no -r * ec2-user@$EC2_IP:$APP_DIR

                    ssh -o StrictHostKeyChecking=no ec2-user@$EC2_IP "
                        sudo yum install -y nodejs npm
                        cd $APP_DIR
                        npm install
                        sudo pkill node || true
                        sudo node app.js &
                    "
                    '''
                }
            }
        }

    }
}
