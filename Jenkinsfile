pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'a3c1693a-a640-4cc9-b251-e773ff372479'
        AZURE_APP_NAME = 'inven'
        RESOURCE_GROUP = 'InventoryManagement'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // SCM checkout is managed by Jenkins automatically
                // Ensure the Jenkins job is configured to use the SCM repository
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Ensure 'npm' is available in the environment
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Package') {
            steps {
                // Create 'deploy' directory and package build artifacts
                sh 'mkdir -p deploy'
                sh 'zip -r deploy/app.zip build/*'
            }
        }
        stage('Deploy to Azure') {
            steps {
                // Deploy the packaged application to Azure App Service
                azureWebAppPublish azureCredentialsId: "${env.AZURE_CREDENTIALS_ID}",
                                   resourceGroup: "${env.RESOURCE_GROUP}",
                                   appName: "${env.AZURE_APP_NAME}",
                                   filePath: 'deploy/app.zip'
            }
        }
    }
}
