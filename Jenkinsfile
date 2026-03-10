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
                sshpass -p '${VM_PASS}' ssh -o StrictHostKeyChecking=no ${VM_USER}@${VM_IP} "
                sudo apt update -y &&
                sudo apt install -y git &&
                curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - &&
                sudo apt install -y nodejs &&
                sudo npm install -g pm2 &&

                if [ ! -d nodeapp ]; then
                    git clone ${REPO_URL} nodeapp;
                fi &&

                cd nodeapp &&
                git pull origin main &&
                npm install &&
                pm2 restart nodeapp || pm2 start app.js --name nodeapp
                "
                """
            }
        }

    }
}
