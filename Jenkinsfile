pipeline {
    agent any
    environment {
        EMAIL_RECIPIENTS = 'royarunava111@gmail.com,sulataroy1111@gmail.com'
    }
    stages {
        stage('Detect Merge') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo "Running on main branch. Checking for merge commit..."
                    def isMergeCommit = sh(script: "git log -1 --pretty=%P | wc -w", returnStdout: true).trim() != "1"
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
                }
            }
        }
    }
}
