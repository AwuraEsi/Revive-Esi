pipeline {
    agent any

    stages {
        stage('create a directory') {
            steps {
                sh '''
                mkdir banana
                rm -rf banana
                '''
            }
        }
        stage('create a file') {
            steps {
                sh '''
                touch banana.txt
                rm -rf banana.txt
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}