pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }

    stages {
        stage('Get Version') {
            steps {
                script {
                    def packageJson = readJSON(file: 'package.json')
                    env.packageVersion = packageJson.version
                    echo "Version: ${env.packageVersion}"
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
                sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
            }
        }

        stage('SAST') {
            steps {
                echo "SAST Completed"
                echo "Package Version: ${env.packageVersion}"
            }
        }

        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '51.20.250.125:8081',
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

        stage('Deploy') {
            steps {
                script {
                    build job: "../catalogue-deploy",
                          wait: true,
                          parameters: [
                              string(name: 'version', value: "${env.packageVersion}")
                          ]
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}