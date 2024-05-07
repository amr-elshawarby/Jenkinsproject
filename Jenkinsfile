pipeline {
    agent any

    parameters {
        choice(name: 'run_build', choices: ['yes', 'no'], description: 'Set to "yes" to run the pipeline, "no" to print an error message.')
    }

    stages {
        stage('Check Run Flag') {
            steps {
                script {
                    if (params.run_build == 'yes') {
                        echo 'Pipeline will run.'
                    } else {
                        error 'Pipeline will not run. Set run_build parameter to "yes" to run the pipeline.'
                    }
                }
            }
        }
        stage('Build and Test') {
            when {
                expression {
                    params.run_build == 'yes'
                }
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
                expression {
                    params.run_build == 'yes'
                }
            }
            steps {
                echo 'Deploying....'
            }
        }
        
        stage('Archive') {
            when {
                allOf {
                    expression {
                        params.run_build == 'yes'
                    }
                    expression {
                        currentBuild.result == 'SUCCESS'
                    }
                }
            }
            steps {
                archiveArtifacts artifacts: 'amr.txt', allowEmptyArchive: true
            }
        }
    }
    
    post {
        always {
            script {
                if (params.run_build == 'yes') {
                    emailext (
                        to: 'amm@example.com',
                        subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                        body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}. Artifacts have been archived successfully."
                    )
                }
            }
        }
        unstable {
            script {
                if (params.run_build == 'yes') {
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

