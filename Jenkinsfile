
pipeline {
    agent any

    stages {

        stage('Remove Old Snap Docker') {
            steps {
                sh '''
                    if snap list docker >/dev/null 2>&1; then
                        sudo snap remove docker
                    fi
                '''
            }
        }

        stage('Update Ubuntu') {
            steps {
                sh '''
                    sudo apt update
                    sudo apt-get install -y docker.io docker-compose-v2
                '''
            }
        }

        stage('Start Docker') {
            steps {
                sh '''
                    sudo systemctl enable --now docker
                '''
            }
        }

        stage('Configure Jenkins Permission') {
            steps {
                sh '''
                    sudo usermod -aG docker jenkins
                '''
            }
        }

        stage('Verify Installation') {
            steps {
                sh '''
                    sudo docker --version
                    sudo docker compose version
                    sudo docker run hello-world
                '''
            }
        }
    }

    post {
        success {
            echo 'APT Docker installation completed!'
        }

        failure {
            echo 'Docker installation failed. Check console output.'
        }
    }
}
