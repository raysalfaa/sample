// pipeline {
//     agent any

//     environment {
//         REPO_URL = 'https://github.com/raysalfaa/sample.git'
//         //RECIPIENTS = 'redeyesinbg@gmail.com'
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 script {
//                     // Print clone URL, base branch, and source branch
//                     echo "Cloning repository from: ${REPO_URL}"

//                     // GitHub PR info (available in environment variables)
//                     def baseBranch = env.BRANCH_NAME // Target branch
//                     def sourceBranch = env.CHANGE_BRANCH // Source branch
//                     echo "Base branch: ${baseBranch}"
//                     echo "Source branch: ${sourceBranch}"

//                     // Check if the target branch is 'main', if not, skip the build
//                     if (baseBranch != 'main') {
//                         echo "Skipping build: Target branch is not 'main'. It's '${baseBranch}'."
//                         currentBuild.result = 'SUCCESS'
//                         return
//                     }

//                     // Checkout the repository
//                     checkout scm
//                 }
//             }
//         }

//         stage('Build') {
//             when {
//                 expression {
//                     // This ensures that the build is triggered only for the 'main' target branch
//                     return env.CHANGE_TARGET == 'main'
//                 }
//             }
//             steps {
//                 echo "Performing build steps for pull request..."
//                 // Insert your build steps here (e.g., compiling, testing)
//             }
//         }
//     }
// }

pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/raysalfaa/sample.git'
        RECIPIENTS = 'redeyesinbg@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Cloning repository from: ${REPO_URL}"

                    // Detect if the job is triggered by a PR
                    def isPullRequest = env.GITHUB_PR_NUMBER ?: false
                    echo "Is this a PR? ${isPullRequest}"

                    // Get branch name dynamically
                    def baseBranch = env.BRANCH_NAME ?: 'unknown'
                    echo "Base branch: ${baseBranch}"

                    ifr(!isPullRequest && baseBranch != 'main') {
                        echo "Skipping build: Not a PR and not 'main' branch."
                        currentBuild.result = 'SUCCESS'
                        return
                    }

                    // Checkout the repository
                    checkout scm
                }
            }
        }

        stage('Build') {
            when {
                expression {
                    return env.BRANCH_NAME == 'main' || env.GITHUB_PR_NUMBER
                }
            }
            steps {
                echo "Performing build steps for branch '${env.BRANCH_NAME}' or PR #${env.GITHUB_PR_NUMBER}"
                // Insert your build steps here (e.g., compiling, testing)
            }
        }
    }
    post {
        failure {
            script {
                emailext (
                    subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: """
                    Hi Team,

                    The Jenkins build '${env.JOB_NAME} #${env.BUILD_NUMBER}' has failed.

                    **Possible Reasons:**
                    - Code Quality Issues
                    - Test Failures
                    - Build Errors

                    **Logs:** ${env.BUILD_URL}/console

                    Regards,  
                    Jenkins CI
                    """,
                    to: RECIPIENTS
                )
            }
        }
    }
}


