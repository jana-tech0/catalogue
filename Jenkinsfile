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
        
        stage('sonar analysis') {
            steps {
                sh 'sonar-scanner'
            }
        }
    }
}