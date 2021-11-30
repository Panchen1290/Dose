pipeline {
    agent any

    environment {
      mainServerImageName = "jalafoundation/dose-main-server"
      contentServerImageName = "jalafoundation/dose-content-server"
      registryCredential = 'jalafoundation-registry'
      dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
          steps {
            git([url: 'https://github.com/jala-bootcamp/Dose.git', branch: 'enable_ci'])
          }
        }
        stage('Build Main Server') {
            steps {
                echo 'Building main server....'
                script {
                  dockerImage = docker.build(mainServerImageName, "./MainServer")
                }
            }
        }
        stage('Publish Main Server') {
            steps {
              script {
                docker.withRegistry( '', registryCredential ) {
                  dockerImage.push("$BUILD_NUMBER")
                   dockerImage.push('latest')
                 }
               }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        stage('Cleanup') {
            steps {
                sh "docker rmi $mainServerImageName:$BUILD_NUMBER"
                sh "docker rmi $mainServerImageName:latest"
            }
        }
    }
}
