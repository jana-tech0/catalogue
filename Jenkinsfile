pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }

    stages {

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit test') {
            steps {
                echo "unit testing is done here"
            }
        }

        stage('Build') {
            steps {
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip . -x "*.git*" -x "*.zip"'
            }
        }
    }
}
