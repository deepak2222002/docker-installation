pipeline {

    agent any

    stages{
        stage("Update Ubuntu") {
            steps{
                sh '''
                        sudo apt update
                        sudo apt-get install -y docker.io docker-compose-v2
                    '''
            }
        }

        stage("Start Docker") {
            steps{
                sh '''
                        sudo systemctl enable docker
                        sudo systemctl start docker

                    '''

            }
        }

        stage("Configure Jenkins Permission"){
            steps{
                sh '''
                    sudo usermod -aG docker jenkins
                    '''
            }
        }

        stage("Verify Installations") {
            steps{
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
            echo 'Docker installation completed successfully!'
        }

        failure {
            echo 'Docker installation failed. Check console output.'
        }
    }

}