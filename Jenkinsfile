pipeline {
    agent any

    environment {
        EMAIL_RECIPIENTS = 'royarunava111@gmail.com'
        GIT_PATH = '"C:\\Program Files\\Git\\bin\\git.exe"'
    }

    stages {
        stage('Detect Merge') {
            steps {
                script {
                    echo "🔍 Checking if HEAD is the latest commit on main..."

                    def fetchOutput = bat(
                        script: "@echo off && ${GIT_PATH} fetch origin main",
                        returnStdout: true
                    )
                    echo "Git fetch output: \n${fetchOutput}"

                    def mainCommit = bat(
                        script: "@echo off && ${GIT_PATH} rev-parse origin/main",
                        returnStdout: true
                    ).trim()

                    def headCommit = bat(
                        script: "@echo off && ${GIT_PATH} rev-parse HEAD",
                        returnStdout: true
                    ).trim()

                    echo "HEAD Commit: ${headCommit}"
                    echo "Main Commit: ${mainCommit}"

                    if (headCommit == mainCommit) {
                        echo "✅ This is the latest commit on main. Checking for merge commit..."

                        def parentHashes = bat(
                            script: "@echo off && ${GIT_PATH} log -1 --pretty=%%P",
                            returnStdout: true
                        ).trim()
                        def parentCount = parentHashes.split().length
                        def isMergeCommit = parentCount > 1

                        if (isMergeCommit) {
                            echo "🟢 Merge commit detected! Sending email..."

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
                                to: "${EMAIL_RECIPIENTS}",
                                mimeType: 'text/plain'
                            )
                        } else {
                            echo "ℹ️ Not a merge commit. No email sent."
                        }
                    } else {
                        echo "❌ HEAD is not the latest commit on main. Skipping."
                    }
                }
            }
        }
    }
}
