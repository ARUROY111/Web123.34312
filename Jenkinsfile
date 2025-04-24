pipeline {
    agent any
    environment {
        EMAIL_RECIPIENTS = 'royarunava111@gmail.com,sulataroy1111@gmail.com'
    }
    stages {
        stage('Detect Merge') {
            steps {
                script {
                    def currentBranch = bat(script: "git rev-parse --abbrev-ref HEAD", returnStdout: true).trim()
                    echo "Current branch: ${currentBranch}"

                    if (currentBranch == 'main') {
                        echo "Running on main branch. Checking for merge commit..."

                        def parentHashes = bat(script: 'git log -1 --pretty=%P', returnStdout: true).trim()
                        def parentCount = parentHashes.split().length
                        def isMergeCommit = parentCount > 1

                        if (isMergeCommit) {
                            echo "✅ This is a merge commit. Sending email..."
                            emailext (
                                subject: "🚀 Merge to main detected!",
                                body: """
Hello Team,

A branch has just been merged into *main*.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

Regards,  
Your Jenkins Bot 🤖
                                """,
                                to: "${EMAIL_RECIPIENTS}"
                            )
                        } else {
                            echo "ℹ️ Not a merge commit. No email sent."
                        }
                    } else {
                        echo "Skipping — not on main branch."
                    }
                }
            }
        }
    }
}