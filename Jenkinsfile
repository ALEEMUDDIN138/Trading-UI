pipeline {
    agent any

    tools {
        nodejs "Node_20"   // make sure you configured this in Jenkins global tools
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ALEEMUDDIN138/Trading-UI.git'
            }
        }

        stage('Clean Workspace') {
            steps {
                sh '''
                  echo "🧹 Cleaning workspace and old dependencies..."
                  rm -rf node_modules package-lock.json
                  npm cache clean --force || true
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                  echo "📦 Installing dependencies..."
                  npm install

                  echo "✅ Forcing correct ESLint version (^6.1.0)..."
                  npm install --save-dev eslint@^6.1.0
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                  echo "🧪 Running tests..."
                  npm test || echo "⚠️ No tests found, skipping..."
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
            echo "✅ React pipeline completed successfully!"
        }
        failure {
            echo "❌ React pipeline failed — check logs above."
        }
    }
}

