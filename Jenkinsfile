pipeline {
    agent { node { label 'agent-1' } }
    environment {
        packageVersion = ''
    }
    stages {
        stage('Get version') {
            steps {
                script {
                    def packageJson = readJSON(file: 'package.json')
                    env.packageVersion = packageJson.version
                    echo "version: ${env.packageVersion}"
                }
            }
        }
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
        stage('Sonar Scan') {
            steps {
                echo "Sonar scan done"
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
                echo "SAST Done"
                echo "package version: ${env.packageVersion}"
            }
        }
        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '13.48.29.89:8081/',
                    groupId: 'com.roboshop',
                    version: "${env.packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
    }
}