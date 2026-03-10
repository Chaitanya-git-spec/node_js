pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/Chaitanya-git-spec/node_js.git"
        VM_IP = "20.62.118.233"
        VM_USER = "chaitanya"
        VM_PASS = "welcome@12345"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Deploy to Azure VM') {
            steps {
                sh """
                sshpass -p '${VM_PASS}' ssh -o StrictHostKeyChecking=no ${VM_USER}@${VM_IP} 

                echo "Updating system packages"
                sudo apt update -y

                echo "Installing Git"
                sudo apt install git -y

                echo "Installing NodeJS"
                curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
                sudo apt install nodejs -y

                echo "Installing PM2 process manager"
                sudo npm install -g pm2

                echo "Cloning application if not present"
                if [ ! -d nodeapp ]; then
                    git clone ${REPO_URL} nodeapp
                fi

                cd nodeapp

                echo "Pulling latest code"
                git pull origin main

                echo "Installing application dependencies"
                npm install

                echo "Starting application"
                pm2 restart nodeapp || pm2 start app.js --name nodeapp
                
                """
            }
        }

    }
}
