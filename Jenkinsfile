pipeline {
    agent any

    stages {
        
        stage('Test microservice catalog') {
          agent {
            docker {
              image 'golang:1.20.1'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/catalog/
            go test
                '''
            }
        }

        stage('Test microservice cart') {
          agent {
            docker {
              image 'maven'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/cart/
            go test
                '''
            }
        }
    }
    post {
       success {
           slackSend color: '#2EB67D',
           channel: 'awuraesi-revive', 
           message: "Revive Project Build Status" +
           "\n Project Name: Revive Project" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : SUCCESS" +
           "\n Build url : ${env.BUILD_URL}"
       }
       failure {
           slackSend color: '#E01E5A',
           channel: 'awuraesi-revive',  
           message: "Revive Project Build Status" +
           "\n Project Name: Revive" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : FAILED" +
           "\n Build User : Tia" +
           "\n Action : Please check the console output to fix this job IMMEDIATELY" +
           "\n Build url : ${env.BUILD_URL}"
       }
       unstable {
           slackSend color: '#ECB22E',
           channel: 'del-uk', 
           message: "Alpha Project Build Status" +
           "\n Project Name: Alpha" +
           "\n Job Name: ${env.JOB_NAME}" +
           "\n Build number: ${currentBuild.displayName}" +
           "\n Build Status : UNSTABLE" +
           "\n Action : Please check the console output to fix this job IMMEDIATELY" +
           "\n Build url : ${env.BUILD_URL}"
       }   
   }
}