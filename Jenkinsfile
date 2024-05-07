pipeline {
    agent any

    parameters {
        choice(name: 'confirmation', choices: ['true', 'false'], description: 'Confirm whether to run the pipeline')
    }

    stages {
        stage('Check Confirmation') {
            when {
                expression {
                    params.confirmation == 'true'
                }
            }
            steps {
                echo 'Pipeline execution confirmed.'
            }
        }

        stage('Error Message') {
            when {
                expression {
                    params.confirmation == 'false'
                }
            }
            steps {
                error('Pipeline execution aborted. User selected "false" to abort.')
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

