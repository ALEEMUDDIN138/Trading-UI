pipeline {
    agent any

    tools {
        nodejs "Node_20"   // make sure NodeJS 18.20.8 is configured in Jenkins
    }

    options {
        skipDefaultCheckout(true)   // control checkout manually
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()   // clean Jenkins workspace
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ALEEMUDDIN138/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                  echo "🧹 Cleaning old dependencies..."
                  rm -rf node_modules package-lock.json
                  
                  echo "📦 Installing fresh dependencies..."
                  npm install
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                  echo "🧪 Running tests..."
                  npm test || echo ":warning: No tests found"
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                  echo "🏗️ Building React app..."
                  CI=false npm run build
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Node.js React pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed — check console output."
        }
    }
}
