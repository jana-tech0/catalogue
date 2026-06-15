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
                    def packageJson = readJSON file: 'package.json'
                    env.packageVersion = packageJson.version

                    echo "================================="
                    echo "Package Version: ${env.packageVersion}"
                    echo "================================="
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
                echo 'Unit Testing Completed'
            }
        }

        stage('Sonar Scan') {
            steps {
                echo 'Sonar Scan Completed'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    rm -f catalogue.zip
                    zip -r catalogue.zip . \
                    -x "*.git*" \
                    -x "*.zip" \
                    -x "node_modules/*"
                '''

                sh 'ls -lh catalogue.zip'
            }
        }

        stage('SAST') {
            steps {
                echo 'SAST Scan Completed'
                echo "Package Version: ${env.packageVersion}"
            }
        }

        stage('Publish Artifact') {
            steps {
                echo "Uploading catalogue-${env.packageVersion}.zip to Nexus"

                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '13.48.29.89:8081',
                    repository: 'catalogue',
                    groupId: 'com.roboshop',
                    version: "${env.packageVersion}",
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
            echo "Build Successful"
        }

        failure {
            echo "Build Failed"
        }

        always {
            cleanWs()
        }
    }
}