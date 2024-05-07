pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                
            }
        }
        stage('Test') {
            steps {
                script {
                    try {
                        echo 'Testing..'
                        sh 'exit 0' 
                    } catch (Exception e) {
                        ansiColor('red') {
                            echo "Test failed"
                            currentBuild.result = 'UNSTABLE'
                            error "Test failed"
                        }
                    }
                }
            }
        }
    }
    
    post {
        success {
            script {
                emailext (
                    to: 'email@example.com',
                    subject: "Pipeline Successful: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: "Congratulations! Your pipeline ${env.JOB_NAME} with build number ${env.BUILD_NUMBER} has successfully completed.",
                )
            }
        }
        failure {
            script {
                emailext (
                    to: 'mail@example.com',
                    subject: "Pipeline Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: "Sorry! Your pipeline ${env.STAGE_NAME} with build number ${env.BUILD_NUMBER} has failed. Please check the Jenkins console output for more details.",
                )
            }
        }
unstable {
            emailext (
                to: 'another_recipient@example.com',
                subject: "Build ${currentBuild.fullDisplayName} ${currentBuild.result}",
                body: "Build ${currentBuild.fullDisplayName} finished with result: ${currentBuild.result}",
            )
 }
    }
}
