pipeline {
    agent any
    
    environment {
        PYTHONUNBUFFERED = '1'
    }

    stages {
        // ==========================================
        //         DEV PIPELINE (Trigger: dev branch)
        // ==========================================
        stage('DEV: Data Ingest & Model Train') {
            when { branch 'dev' }
            steps {
                echo "DEV ENVIRONMENT TRIGGERED"
                sh 'pip install -r requirements.txt'
                sh 'python src/train.py'
            }
        }
        stage('DEV: Model Deploy & Test') {
            when { branch 'dev' }
            steps {
                echo "Deploying to MLflow and Testing..."
            }
            post {
                success {
                    echo "SUCCESS: Saving model in registry (mlflow) - assign alias 'Challenger'"
                }
                failure {
                    echo "FAILED: Notifying through Email"
                }
            }
        }

        // ==========================================
        //  PRE-PROD PIPELINE (Trigger: main branch)
        // ==========================================
        stage('PRE-PROD: Load & Register') {
            when { branch 'main' }
            steps {
                echo "PRE-PROD ENVIRONMENT TRIGGERED"
                echo "Loading the model (Challenger alias)"
                echo "Logging and Registering the model - assign alias 'Challenger-pre-test'"
            }
        }
        stage('PRE-PROD: Deploy & Test') {
            when { branch 'main' }
            steps {
                echo "Model Deploy (mlflow)"
                echo "Model Test"
            }
            post {
                success {
                    echo "SUCCESS: Update alias of model - 'Challenger-post-test'"
                }
                failure {
                    echo "FAILED: Notifying through Email"
                }
            }
        }

        // ==========================================
        //    PROD PIPELINE (Trigger: Release Tag)
        // ==========================================
        stage('PROD: Load & Register') {
            when { buildingTag() }
            steps {
                echo "PROD ENVIRONMENT TRIGGERED"
                echo "Load the model (Challenger-post-test alias)"
                echo "Log and Register the model - assign alias 'Champion'"
            }
        }
        stage('PROD: Deploy') {
            when { buildingTag() }
            steps {
                echo "Model Deploy (mlflow)"
                echo "Production Deployment Complete!"
            }
        }
    }
}