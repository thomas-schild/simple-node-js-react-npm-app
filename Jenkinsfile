pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    // the 'npm test' called via test.sh reacts to the env var CI and sets npm in non-watch mode, otherwise the npm is in watch mode and indeed expects user input which can not be entered in the Jenkins executer.
    // alternatve to set the env var here is to fiddel around with the test.sh and package.json file
    environment {
        CI = 'true' 
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
