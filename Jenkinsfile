pipeline {
    agent any
  tools {
  nodejs 'NodeJS-20.4.0'
}
    environment {
      DOCKER_TAG = getVersion()
    }

    stages {
        stage('CLEAN WORKSPACE') {
            steps {
                cleanWs()
            }
        }

        stage('CHECKOUT CODE') {
            steps {
                git 'https://github.com/Praveeen1996/node-app.git'
            }
        }
        stage('NODEJS BUILD CODE') {
            steps {
                sh 'npm install'
            }
        }
        stage('DOCKER BUILD'){
            steps{
                sh "docker build . -t praveenhema/nodeapp:${DOCKER_TAG} "
            }
        }
        stage('DOCKERHUB PUSH'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u praveenhema -p ${dockerHubPwd}"
                }
                
                sh "docker push praveenhema/nodeapp:${DOCKER_TAG} "
            }
        }
        
    }
}
def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
