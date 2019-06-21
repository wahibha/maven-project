pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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