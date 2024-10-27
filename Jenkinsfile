pipeline {
    agent any

    environment {
        VENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Build') {
            steps {
                sh 'python3 -m venv $VENV'
                sh '$VENV/bin/pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh '$VENV/bin/pytest'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to staging environment...'
                // Add deployment commands here, e.g., Docker, SCP, or any other deployment method.
            }
        }
    }

    post {
        success {
            mail to: 'you@example.com',
                 subject: "SUCCESS: Build #${env.BUILD_NUMBER}",
                 body: "The build ${env.BUILD_NUMBER} succeeded."
        }
        failure {
            mail to: 'you@example.com',
                 subject: "FAILURE: Build #${env.BUILD_NUMBER}",
                 body: "The build ${env.BUILD_NUMBER} failed."
        }
    }
}
