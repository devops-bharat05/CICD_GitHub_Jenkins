pipeline {
    agent { label 'Jenkins-worker01' }

    environment {
        VENV_PATH = "venv" // Path to the virtual environment
    }
    stages {
        stage('Build') {
            steps {
                // Install dependencies in a virtual environment
                sh 'python3 -m venv $VENV_PATH'
                sh './$VENV_PATH/bin/pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                // Run unit tests
                sh './$VENV_PATH/bin/pytest test/'
            }
        }
        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                // Deploy to staging (example: simple gunicorn command)
                script {
                    sh """
                    sudo cp myflaskapp.service /etc/systemd/system/myflaskapp.service
                    sudo systemctl daemon-reload
                    sudo systemctl start myflaskapp
                    sudo systemctl enable myflaskapp
                    sudo systemctl status myflaskapp
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
