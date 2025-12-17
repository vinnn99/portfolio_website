pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                echo 'Validating project structure...'
                script {
                    if (!fileExists('index.html')) {
                        error 'ERROR: index.html not found! Portfolio must have index.html at root.'
                    }
                    echo '✓ index.html found'
                }
            }
        }

        stage('Display Structure') {
            steps {
                echo 'Displaying project structure...'
                sh '''
                    echo "=== Portfolio Structure ==="
                    find . -type f \\( -name "*.html" -o -name "*.css" -o -name "*.js" \\) | sort
                    echo ""
                    echo "=== Directory Tree ==="
                    tree -L 2 --charset ascii 2>/dev/null || find . -not -path '*/.*' -not -path '*/.git/*' -type d | sort
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving portfolio files...'
                archiveArtifacts artifacts: '**/*.html,**/*.css,**/*.js,**/*.json,assets/**,fonts/**',
                                  allowEmptyArchive: false,
                                  fingerprint: true
                echo 'Portfolio packaged successfully!'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Portfolio validated and archived successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check validation errors.'
        }
    }
}
