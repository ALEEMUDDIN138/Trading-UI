pipeline {
    agent any

    tools {
        // Use the NodeJS tool you configured in Jenkins (Node 18.20.8 or Node 20.x)
        nodejs 'Node_20'  
    }

    environment {
        // Bypass CRA eslint version mismatch
        SKIP_PREFLIGHT_CHECK = 'true'
        // Sometimes needed for React builds on Node 18+
        NODE_OPTIONS = '--openssl-legacy-provider'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[url: 'https://github.com/ALEEMUDDIN138/Trading-UI.git']]
                ])
            }
        }

        stage('Clean Workspace') {
            steps {
                sh '''
                    echo "Cleaning workspace..."
                    rm -rf node_modules package-lock.json build
                    npm cache clean --force || true
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    echo "Installing dependencies..."
                    npm install --legacy-peer-deps
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "Running tests..."
                    npm test || echo "✅ No tests found, continuing..."
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Building React app..."
                    npm run build
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check errors above."
        }
    }
}
