pipeline {
    agent { label 'agent-node' }
    stages {
        stage('Continuous Integration') {
            steps {
                git branch: 'main', url: 'https://github.com/alexxbx/TP_Frontend_deployment_cda'
            }
        }
        stage("controle qualité") {
            steps {
                sh '''
                    sonar-scanner \
                        -Dsonar.projectKey=alex-tp-frontend \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://669b-212-114-26-208.ngrok-free.app \
                        -Dsonar.token=sqp_ac71ceab55e0dda34c12211d7b8808b970d59ac4
                '''
            }
        }
        stage('Deploy via FTP') {
            steps {
                sh '''
                    lftp -d -u alex1,Jpbond92500+ ftp-alex1.alwaysdata.net -e "
                        mirror -R -X node_modules/ -X .git/ /home/jenkins/workspace/alex-tp-frontend/ www/;
                        bye
                    "

                '''
            }
        }
        stage('Install Node') {
            steps {
                sh """
                    sshpass -p ${sshpassword} ssh -o StrictHostKeyChecking=no alex1@ssh-alex1.alwaysdata.net '
                        cd ~/www && npm install --no-dev &&
                        echo "API_URL=https://alex2.alwaysdata.net/api/podcast.php" > .env
                    '
                """
            }
        }
    }
}