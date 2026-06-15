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

        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '13.48.29.89:8081',
                    groupId: 'com.roboshop',
                    version: '1.0.1',
                    repository: 'catalogue',
                    credentialsId: 'nexus',
                    artifacts: [
                        [
                            artifactId: 'catalogue',
                            classifier: '',
                            file: 'catalogue.zip',
                            type: 'zip'
                        ]
                    ]
                )
            }
        }
    }
}
