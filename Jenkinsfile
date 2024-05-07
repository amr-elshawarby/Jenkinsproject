pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                // Add build steps here
            }
        }
        stage('Test') {
            steps {
                script {
                    try {
                        echo 'Testing..'
                        sh 'exit 1'
                    } catch (Exception e) {
                        ansiColor('red') {
                            echo "Test failed"
                            currentBuild.result = 'UNSTABLE'
                        }
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying..'
            }
        }
    }
}

