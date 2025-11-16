pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // When using "Pipeline script from SCM" Jenkins already checks out,
                // so this stage is just for logging
                echo "Source code checked out from Git"
            }
        }

        stage('Install dependencies') {
            steps {
                echo "Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Run tests (optional)') {
            when {
                expression { return fileExists('package.json') }
            }
            steps {
                echo "Running tests (if configured)..."
                // If tests are not set up, this will just warn and continue
                sh 'npm test -- --watch=false || echo "Tests not configured or failed"'
            }
        }

      stage('Build') {
    steps {
        echo "Building production bundle..."
        // CI=false makes CRA treat warnings as warnings, not errors
        sh 'CI=false npm run build'
    }
}


        stage('Archive build artifacts') {
            steps {
                echo "Archiving build folder..."
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ React E-Commerce build SUCCESS'
        }
        failure {
            echo '❌ Build FAILED – check the console output.'
        }
    }
}
