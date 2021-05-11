pipeline {
    agent any

    stages {
        stage('Build') {
            agent { label 'slave' }
            steps {
                echo 'maven build..'
                sh '''
                   sudo mvn compile
                '''
            }
        }
        stage('TEST') {
            agent { label 'slave' }
            steps {
                echo 'package..'
                sh '''
                   sudo mvn package
                '''
            }
        }
        stage('DEPLOY') {
            agent { label 'slave' }
            steps {
                echo 'deploying..'
                sh '''
                   sudo mvn tomcat:redeploy
                '''
            }
        } 
    }		
    post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "duvva.raghavendra@gmail.com",
                sendToIndividuals: true])
        }     
    }       
}
