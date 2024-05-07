pipeline {
    agent any

    stages {
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
        
        stage('Archive') {
            when {
                allOf {
                    previousBuild().result == 'SUCCESS'
                    currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                archiveArtifacts artifacts: 'amr.txt', allowEmptyArchive: true
            }
        }
    }
    
    post {
        always {
            emailext (
                to: 'amm@example.com',
                subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}. Artifacts have been archived successfully."
            )
        }
        unstable {
            emailext (
                to: 'amm@example.com',
                subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}."
            )
        }
    }
}

