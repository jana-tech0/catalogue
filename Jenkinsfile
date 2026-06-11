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
        
        // stage('sonar analysis') {
        //     steps {
        //         sh 'sonar-scanner'
        //     }
        // }

        stages('build') {
            steps {
                echo "build is done here"
            }
        }
        stages('deploy') {
            steps {
                echo "deploy is done here"
            }
        }
    }

    post {
        always {
            echo "clearning the workspace after the build"
            deleteDir()
        }
    }
}