pipeline {
    agent any
    stages {
        stage('Clone repository') { 
            steps {
              git (
                branch: 'main',
                url: 'git@github.com:Talits/ada-ci.git'
              )
            }
        }

        
        stage('Build dev'){
            when {
               not {
                  branch "main"
               }
            }
            steps { 
                script{
                 image = docker.build("talits/v1:develop")
                 
                }
            }
        }
        stage('Build') {
            when {
                branch "main"
            } 
            steps { 
                script{
                 image = docker.build("talits/v1:main")
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
        stage('Deploy') {
            steps {
                 script {
                    docker.image('talits/v1:main').withRun('-p 3000:3000 -d') {c ->
                        sleep 300
                    }
                }
            }
                
        }
    }
}
