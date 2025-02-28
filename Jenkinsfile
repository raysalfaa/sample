pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQube' // This should match the SonarQube name you configured in Jenkins
        SONARQUBE_SCANNER_HOME = tool name: 'SonarQube Scanner', type: 'Tool' // Tool name from Jenkins configuration
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Example: Build the application (Python or any other language)
                script {
                    // Run your build command, e.g., for Python
                    sh 'python -m unittest discover'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis
                script {
                    def scannerHome = tool name: 'SonarQube Scanner', type: 'Tool'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=my-python-app \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube analysis to complete and check the Quality Gate
                script {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
