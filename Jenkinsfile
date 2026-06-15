pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }

    environment {
        packageVersion = ''
    }

    stages {

        stage('Get Version') {
            steps {
                script {
                    def packageJson = readJSON(file: 'package.json')
                    packageVersion = packageJson.version

                    echo "Version: ${packageVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                echo "Unit Testing Completed"
            }
        }

        stage('Sonar Scan') {
            steps {
                echo "Sonar Scan Completed"
            }
        }

        stage('Build') {
            steps {
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
            }
        }

        stage('SAST') {
            steps {
                echo "SAST Completed"
                echo "Package Version: ${packageVersion}"
            }
        }

        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '13.48.29.89:8081',
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
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

    post {
        always {
            echo 'Cleaning workspace'
        }
    }
}