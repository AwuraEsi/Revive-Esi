pipeline {
    agent any

    stages {
        stage('clean env') {
            steps {
                sh '''
            docker system prune -fa || true
                '''
            }
        }
      
        
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
              image 'maven:3.8.7-openjdk-18'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/cart/
            mvn  test  -Dmaven.test.skip=true --quiet
                '''
            }
        }
        stage('Test microservice orders') {
          agent {
            docker {
              image 'maven:3.8.7-openjdk-18'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/orders/
            mvn  test  -Dmaven.test.skip=true --quiet
                '''
            }
        }
        stage('Test microservice ui') {
          agent {
            docker {
              image 'maven:3.8.7-openjdk-18'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/ui/
            mvn  test  -Dmaven.test.skip=true --quiet
                '''
            }
        }
        stage('Test microservice checkout') {
          agent {
            docker {
              image 'node'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/checkout/
            npm install
          
          
                '''
            }
        }
        stage('SonarQube analysis') {
                agent {
                    docker {
                      image 'sonarsource/sonar-scanner-cli:4.7.0'
                    }
                   }
                   environment {
            CI = 'true'
            scannerHome='/opt/sonar-scanner'
        }
                steps{
                    withSonarQubeEnv('sonar') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        stage("Quality Gate") {
                steps {
                  timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                  }
                }
              }
        stage('build microservice catalog') {
          agent {
            docker {
              image 'golang:1.20.1'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/catalog/
            go build  
                '''
            }
        }    
        stage('build microservice cart') {
          agent {
            docker {
              image 'maven:3.8.7-openjdk-18'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/cart/
            mvn  package -Dmaven.test.skip=true --quiet
                '''
            }
        }  stage('build microservice checkout') {
          agent {
            docker {
              image 'node'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/checkout/
            npm run build
          
          
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