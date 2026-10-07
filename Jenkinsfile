pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building the application...'

                bat 'echo Compiling source code'

                bat '''
                    if not exist build mkdir build
                    echo build artifact > build\\app.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'

                bat 'echo Running unit tests'

                bat '''
                    if exist build\\app.txt (
                        echo Test passed: build artifact exists
                    ) else (
                        echo Test failed: build artifact does not exist
                        exit /b 1
                    )
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'

                bat '''
                    if not exist deploy mkdir deploy
                    copy /Y build\\app.txt deploy\\app.txt
                '''

                bat '''
                    echo Deployed at %DATE% %TIME% > deploy\\deployment.log
                '''

                bat 'echo Deployment complete. Files in deploy:'

                bat 'dir deploy'

                bat 'echo ---- deployment.log ----'

                bat 'type deploy\\deployment.log'
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully! Application deployed.'
        }

        failure {
            echo 'Pipeline failed. Deployment aborted.'
        }

        always {
            echo "Build #${env.BUILD_NUMBER} finished with status: ${currentBuild.currentResult}"
        }
    }
}
