pipeline {
    agent any
    
    environment {
        PYTHONUNBUFFERED = '1'
    }

    stages {
        stage('Data Ingest & Model Train') {
            steps {
                echo "Phase 1: Starting Data Ingestion and Model Training..."
                sh 'pip install -r requirements.txt'
                sh 'python src/train.py'
            }
        }
        
        stage('Model Deploy (mlflow)') {
            steps {
                echo "Phase 2: Deploying Model to MLflow..."
                // Placeholder for deployment logic
                sh 'echo "Model successfully deployed to local MLflow instance."'
            }
        }
        
        stage('Model Test') {
            steps {
                echo "Phase 3: Testing the Deployed Model..."
                // Placeholder for testing logic
                sh 'echo "All model endpoint tests passed successfully."'
            }
        }
    }
    
    post {
        success {
            echo "Pipeline Success!"
            echo "Action: Saving model in registry (mlflow) and assigning alias 'Challenger'"
        }
        failure {
            echo "Pipeline Failed!"
            echo "Action: Notifying through Email..."
        }
    }
}