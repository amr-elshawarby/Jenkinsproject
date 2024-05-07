pipeline {
    agent any

    parameters {
        booleanParam(name: 'confirmation', defaultValue: false, description: 'Set to true to confirm running the pipeline')
    }

    stages {
        stage('User Confirmation') {
            steps {
                script {
                    if (!params.confirmation) {
                        error('Pipeline execution aborted. Please set "confirmation" parameter to true to confirm running the pipeline.')
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

