pipeline {
  agent any

  tools {nodejs "node"}

  stages {
    stage('Cloning Git') {
      steps {
        slackSend (color: '#FFFF00', message: "INICIO: Tarea '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        git 'https://github.com/Diegoxlus/node-jenkins'
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'npm install'
        sh 'npm install --save-dev chai'
        slackSend (color: '#00FF00', message: "Instalación de dependencias")
      }
    }
     
    stage('Test') {
      steps {
         sh 'npm test'
         slackSend (color: '#00FF00', message: "Tests")
      }
    }

    stage('SonarQube analysis') {
        steps {
            script {
                scannerHome = tool 'SonarQube Scanner 4.6.0.2311'
            }
            withSonarQubeEnv('SonarQube') {
            sh 'ls'
            sh 'cd node-jenkins'
            sh "${scannerHome}/bin/sonar-scanner"
            }
        }
    }

  }
  post {
    success{
        slackSend (color: '#00FF00', message: ":white_check_mark:INTEGRACIÓN CORRECTA :white_check_mark:")
    }
    failure {
        slackSend (color: '#00FF00', message: ":red_circle: INTEGRACIÓN INCORRECTA :red_circle:")
    }
  }
}
