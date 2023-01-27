pipeline {
    agent {label 'worker'}
    stages {
        stage('Clone repository') { 
            steps {
              git (
                branch: 'main',
                credentialsId: '5b5df9af-8d67-4442-9335-71de7960da54',
                url: 'git@github.com:Talits/ada-ci.git'
              )
            }
        }

        stage('Build') {
            when {
                branch 'origin/main'
            } 
            steps { 
                script{
                 image = docker.build("talits/v1:main")
                }
            }
        }
        stage('Build dev'){
            when {
               not {
                  branch 'origin/main'
               }
            }
            steps { 
                script{
                 image = docker.build("talits/v1:develop")
                }
            }
        }
        stage('Push') {
            steps {
                script{
                       docker.withRegistry('', 'docker') {
                       image.push()
                    }
                }
            }
        }
    }
}
