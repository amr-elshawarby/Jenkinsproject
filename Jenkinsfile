pipeline {
    agent any
    
    parameters {
        choice(name: 'confirmation', choices: ['true', 'false'], description: 'Confirm whether to run the pipeline')
    }
    
    stages {
        stage('Check Confirmation') {
            steps {
                script {
                    if (params.confirmation == 'false') {
                        error('Pipeline execution aborted. User selected "false" to abort.')
                    }
                }
            }
        }
        
        stage('SCM Checkout') {
            when {
                expression { params.confirmation == 'true' }
            }
            steps {
                checkout scm
            }
        }
        
        stage('Build and Test') {
            when {
                expression { params.confirmation == 'true' }
            }
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
            when {
                expression { params.confirmation == 'true' }
            }
            steps {
                echo 'Deploying....'
            }
        }
    }
    
    post {
        always {
            script {
                if (params.confirmation == 'false') {
                    echo 'Printing error message...'
                    echo 'Pipeline execution aborted. User selected "false" to abort.'
                }
                else {
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
        }
    }
}

