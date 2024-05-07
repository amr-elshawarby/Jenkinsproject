pipeline {
    agent any

    stages {
        stage('User Confirmation') {
            steps {
                script {
                    
                    def userInput = input(
                        id: 'confirm',
                        message: 'Do you want to run this pipeline? (Enter "true" to proceed or "false" to abort)',
                        parameters: [booleanParam(defaultValue: false, description: 'Enter true to proceed or false to abort')]
                    )

                    
                    if (!userInput) {
                        error('Pipeline execution aborted. User selected "false" to abort.')
                    }
                }
            }
        }

        stage('Build and Test') {
            parallel {
                stage('Build') {
                    steps {
                        echo 'Building..'
                        writeFile file: 'amr.txt', text: 'hello amr'
                    }
                }
                stage('Test') {
                    steps {
                        echo 'Testing..'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    
    post {
        success {
            script {
                if (currentBuild.result == 'SUCCESS') {
                    echo 'Archiving....'
                    archiveArtifacts artifacts: 'amr.txt', allowEmptyArchive: true
                    emailext (
                        to: 'amm@example.com',
                        subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                        body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}. Artifacts have been archived successfully."
                    )
                } else {
                    emailext (
                        to: 'amm@example.com',
                        subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                        body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}."
                    )
                }
            }
        }
        always {
            emailext (
                to: 'amm@example.com',
                subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}."
            )
        }
    }
}

