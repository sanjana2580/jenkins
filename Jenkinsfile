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
                sh 'docker run -it -d --name sanjana -p 8000:80 nginx'
            }
        }
        stage('backup'){
            steps{
                sh 'docker commit sanjana backup'
            }
        }
        stage('tag'){
            steps{
                sh 'docker tag backup sanjana225/mock'
            }
        }
        stage('push'){
            steps{
                sh 'echo "Biradar@24" | docker login -u "sanjana225" --password-stdin'
                sh 'docker push sanjana225/mock'
            }
        }
    }
    post{
        always{
            sh 'docker rmi -f nginx'
        }
        success{
            sh 'docker rm -f sanjana'
        }
    }
}