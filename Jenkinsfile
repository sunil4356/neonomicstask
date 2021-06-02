pipeline{
  agent any

  stages{
    stage('SCM'){
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
            sh 'docker build . -t sunil4356/neonomicsapp:0.0.1 '
        }
    }
    stage('DockerHub Push'){
        steps{
            withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                sh "docker login -u sunil4356 -p ${dockerHubPwd}"
            }
            sh 'docker push sunil4356/neonomicsapp:0.0.1'
        }
    }
    stage('Docker Deploy'){
        steps{
            ansiblePlaybook credentialsId: 'neo-deploy', disableHostKeyChecking: true, installation: 'ansible', inventory: 'neo.inv', playbook: 'deploy-docker.yml'
        }
    }
  }
}
