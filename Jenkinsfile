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
        //}

        stage('build') {
           steps {
               sh 'ls -ltr'
               sh 'zip -r ./* --exclude=.git --exclude=.zip'
            }
        }

        stage('deploy') {
            steps {
                echo "deploy is done here"
            }
        }
    }

     post {
        always {
            echo "cleaning the workspace after the build"
            deleteDir()
         }
    }
}