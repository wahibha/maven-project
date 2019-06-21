pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
                bat "docker build . -t jenkinsimage:${env.BUILD_ID}"
                bat "docker run --name webapp -t jenkinsimage:${env.BUILD_ID} -p 8081:8080"
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Static-chekstyle'){
            steps{
                build job: 'static-checkstyle'
            }
        }
        stage('Deploy to staging'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message : 'Approve Stagging Deployement' 
                }
                build job: 'Deploy-to-staging'
            }
            post{
                success{
                    echo 'Deploy to Staging success'
                }
                failure{
                    echo 'Deployement fail'
                }
            }
        }
    }
}