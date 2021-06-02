pipeline{
    agent any
    environment {
      DOCKER_TAG = getVersion()
    }

    stages{
        stage ('SCM Checkout'){
            steps{
                git credentialsId: 'github', url: 'https://github.com/msunilkumar/neonomicstask.git'
            }
        }
        stage('Maven Build'){
            steps{
                sh "mvn clean compile package"
            }
        }
        stage('Docker Build'){
            steps{
                sh 'docker build . -t sunil4356/neonomicsapp:${DOCKER_TAG} '
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u sunil4356 -p ${dockerHubPwd}"
                }
                sh 'docker push sunil4356/neonomicsapp:${DOCKER_TAG}'
            }
        }
        stage('Decker Deploy'){
            steps{
                ansiblePlaybook credentialsId: 'neo-deploy', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'neo.inv', playbook: 'deploy-docker.yml'
            }
        }
    }
}
def getVersion(){
    def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
