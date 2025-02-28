hpipeline {
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
                    echo "${baseBranch}"
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
        // failure {
        //     // Send an email when the build fails
        //     // emailext(
        //     //     subject: "Build Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
        //     //     body: """
        //     //         The build failed for the project: ${env.JOB_NAME}#${env.BUILD_NUMBER}.
                    
        //     //         <h3>Build Details:</h3>
        //     //         <ul>
        //     //             <li>Project: ${env.JOB_NAME}</li>
        //     //             <li>Build: ${env.BUILD_NUMBER}</li>
        //     //             <li>Cause: ${currentBuild.result}</li>
        //     //             <li>Branch: ${env.BRANCH_NAME}</li>
        //     //             <li>Build URL: ${env.BUILD_URL}</li>
        //     //         </ul>
        //     //     """,
        //     //     to: "${redeyesinbg@gmail.com}"
        //     // )
        // }
        always {
            echo "Build completed for PR: ${env.CHANGE_ID}."
        }
    }
}
