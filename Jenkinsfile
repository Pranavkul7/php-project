pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Pranavkul7/php-project.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Tagging using lowercase repo name for Docker Hub
                    sh 'docker build -t pranavkul07/akshatnewimg6july:v1 .'
                    sh 'docker images'
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push pranavkul07/akshatnewimg6july:v1'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def containerName = "My-first-containe2211"
                    def imageName = "pranavkul07/akshatnewimg6july:v1"
                    def dockerRmCmd = "sudo docker rm -f ${containerName} || true"
                    def dockerRunCmd = "sudo docker run -itd --name ${containerName} -p 8083:80 ${imageName}"

                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.37.20 '${dockerRmCmd}'"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.37.20 '${dockerRunCmd}'"
                    }
                }
            }
        }
    }
}
