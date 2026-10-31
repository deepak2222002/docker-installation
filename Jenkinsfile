
pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
    }

    stages {

        stage('Remove Old Snap Docker') {
            steps {
                sh '''
                    if sudo snap list docker >/dev/null 2>&1; then
                        echo "Removing old Snap Docker..."
                        sudo snap remove docker
                    else
                        echo "Snap Docker is not installed."
                    fi
                '''
            }
        }

        stage('Update Ubuntu') {
            steps {
                sh '''
                    sudo apt-get update
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
            echo 'Fresh APT Docker installation completed!'
            echo 'Restart Jenkins service after this pipeline completes to apply group changes.'
        }

        failure {
            echo 'Docker installation failed. Check console output.'
        }
    }
}
