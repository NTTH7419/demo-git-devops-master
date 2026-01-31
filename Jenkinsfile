pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup Environment') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            python3 -m venv venv
                            . venv/bin/activate
                            pip install pytest
                        '''
                    } else {
                        bat '''
                            python -m venv venv
                            call venv\\Scripts\\activate.bat
                            pip install pytest
                        '''
                    }
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            . venv/bin/activate
                            pytest test_*.py -v
                        '''
                    } else {
                        bat '''
                            call venv\\Scripts\\activate.bat
                            pytest test_*.py -v
                        '''
                    }
                }
            }
        }
    }
    
    post {
        always {
            script {
                if (isUnix()) {
                    sh 'rm -rf venv'
                } else {
                    bat 'if exist venv rmdir /s /q venv'
                }
            }
        }
    }
}