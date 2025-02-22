pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshvardhan011-k/jenkinspipeline.git' // Replace with your repo
            }
        }
        stage('Setup Environment') {
            steps {
                sh '''
                #!/bin/bash
                python3 -m venv venv  # Create virtual environment
                . venv/bin/activate    # Activate it
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                #!/bin/bash
                . venv/bin/activate
                python3 src/main.py  # Use python3 explicitly
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                #!/bin/bash
                . venv/bin/activate
                pytest src/test_main.py --junitxml=test-results.xml
                '''
            }
            post {
                always {
                    junit 'test-results.xml' // Archive test results
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
            cleanWs() // Clean workspace after pipeline execution
        }
        failure {
            echo "Pipeline failed! Check logs for more details."
        }
    }
}
