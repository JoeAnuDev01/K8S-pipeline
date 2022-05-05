pipeline { 
    agent any 
    tools {
      maven 'Maven home'
        }
    environment {
      DOCKER_TAG = getVersion()
    }    
    stages {
        stage('SCM-Checkout') { 
            steps { 
               git branch: 'main', credentialsId: 'Git-credentials', 
                   url: 'https://github.com/JoeAnuDev01/K8S-pipeline.git' 
            }
        }
        stage('Maven-Build'){
            steps {
                sh 'mvn clean compile package'
            }
        }
        stage('Docker-build'){
            steps {
                sh 'docker build . -t joeanu7577/nodeapp:${DOCKER_TAG}'
            }
        }
        stage('Docker-Push'){
            steps {
              withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerHubPwd')]) {
                sh 'docker login -u joeanu7577 -p ${dockerHubPwd}'
              }
            sh 'docker push joeanu7577/nodeapp:${DOCKER_TAG}'
            }
        }
      }        
    }

def getVersion(){
    def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
