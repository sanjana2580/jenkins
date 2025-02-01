pipeline {
    agent any

    stages {
        stage('Install Docker') {
            steps {
                script {
                    def dockerInstalled = sh(script: 'which docker', returnStatus: true) == 0
                    if (!dockerInstalled) {
                        sh '''
                        sudo yum update -y
                        sudo yum install -y docker
                        sudo systemctl start docker
                        sudo systemctl enable docker
                        sudo usermod -aG docker jenkins
                        '''
                    }
                }
            }
        }

        stage('pull'){
            steps{ 
                sh 'docker pull nginx'
            }
        }
        stage('run'){
            steps{
                sh 'docker run -it -d --name chandu -p 8000:80 nginx'
            }
        }
        stage('backup'){
            steps{
                sh 'docker commit chandu backup'
            }
        }
        stage('tag'){
            steps{
                sh 'docker tag backup kulashekaralwarn/mock'
            }
        }
        stage('push'){
            steps{
                sh 'docker push kulashekaralwarn/mock'
            }
        }
    }
    post{
        always{
            sh 'docker rmi -f nginx'
        }
        success{
            sh 'docker rm -f chandu'
        }
    }
}