pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/raysalfaa/sample.git'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Print clone URL, base branch, and source branch
                    echo "Cloning repository from: ${REPO_URL}"

                    // GitHub PR info (available in environment variables)
                    def baseBranch = env.CHANGE_TARGET // Target branch
                    def sourceBranch = env.CHANGE_BRANCH // Source branch
                    
                    // Check if the target branch is dev, if not, skip the build
                    if (baseBranch != 'main') {
                        echo "Skipping build: Target branch is not 'dev'. It's '${baseBranch}'."
                        currentBuild.result = 'SUCCESS'
                        return
                    }

                    echo "Base branch: ${baseBranch}"
                    echo "Source branch: ${sourceBranch}"

                    // Checkout the repository
                    checkout scm
                }
            }
        }

        stage('Build') {
            when {
                expression {
                    // This ensures that the build is triggered only for the 'dev' target branch
                    return env.CHANGE_TARGET == 'main'
                }
            }
            steps {
                echo "Performing build steps for pull request..."
                // Insert your build steps here (e.g., compiling, testing)
            }
        }
    }

    post {
        always {
            echo "Build completed for PR: ${env.CHANGE_ID}."
        }
    }
}
