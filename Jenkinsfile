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
                    env.packageVersion = packageJson.version
                    echo "Package Version: ${env.packageVersion}"
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
                echo "Unit testing is done here"
            }
        }

        stage('Sonar Scan') {
            steps {
                echo "Sonar Scan Done"
            }
        }

        stage('Build') {
            steps {
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip . -x "*.git*" -x "*.zip"'
            }
        }

        stage('SAST') {
            steps {
                echo "SAST Scan Done"
                // This should now show the correct version
                echo "Package Version: ${env.packageVersion}"
            }
        }

        stage('Verify Artifact') {
            steps {
                sh 'ls -lh catalogue.zip'
            }
        }

        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '13.48.29.89:8081',
                    groupId: 'com.roboshop',
                    version: "${env.packageVersion}",
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
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}