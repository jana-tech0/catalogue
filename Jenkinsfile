pipeline {
    agent { node { label 'agent-1'} }
    environment {
        packageVersion = ''
    }
    stages {
        stage('Get Version') {
            steps {
                script {
                    def gitHash = sh(
                        script: "git log -1 --format=%h",
                        returnStdout: true
                    ).trim()
                    packageVersion = "${env.BUILD_NUMBER}-${gitHash}"
                    echo "version: ${packageVersion}"
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
                echo "package version: $packageVersion"
            }
        }
        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '56.228.9.121:8081',
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
        stage('Deploy') {
            steps {
                script {
                    echo "Deployment"
                    def deployParams = [      
                        string(name: 'version', value: "${packageVersion}")
                    ]
                    build job: "../catalogue-deploy", wait: true, parameters: deployParams
                }
            }
        }
    }
    post {
        always {
            echo 'cleaning up workspace'
            deleteDir()
        }
    }
}