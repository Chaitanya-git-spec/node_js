pipeline {
  agent any
  stages {
    stage('Checkout Code') {
      steps {
        git(branch: 'main', url: 'https://github.com/Chaitanya-git-spec/node_js.git')
      }
    }

    stage('Deploy to EC2') {
      steps {
        sshagent(credentials: ['ccaccess']) {
          sh '''
                    
                    # Create directory on EC2
                    ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_IP "mkdir -p $APP_DIR"

                    # Copy project files
                    scp -o StrictHostKeyChecking=no -r * $EC2_USER@$EC2_IP:$APP_DIR

                    # Install NodeJS and run application
                    ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_IP "
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
  environment {
    EC2_IP = '18.210.28.231'
    EC2_USER = 'ec2-user'
    APP_DIR = '/home/ec2-user/nodeapp'
  }
}
