pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "v1.0.0" // Can be parameterized
        GIT_REPO = "https://github.com/Manjesh501/gitops-register-app.git"
        GIT_BRANCH = "main"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from GitHub") {
            steps {
                // This uses GitHub checkout WITHOUT credentials
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
            }
        }

        stage("Update Deployment Tags") {
            steps {
                sh """
                    echo "Before change:"
                    cat deployment.yaml

                    sed -i "s|${APP_NAME}:.*|${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml

                    echo "After change:"
                    cat deployment.yaml
                """
            }
        }

        stage("Commit and Push Changes") {
            steps {
                withCredentials([string(credentialsId: 'github_pat_token', variable: 'GITHUB_PAT')]) {
                    sh """
                        git config user.name "Manjesh Tiwari"
                        git config user.email "manjesht78@gmail.com"
                        git add deployment.yaml
                        git commit -m "Update image tag to ${IMAGE_TAG}" || echo "No changes to commit"
                        git push https://Manjesh501:${GITHUB_PAT}@github.com/Manjesh501/gitops-register-app.git HEAD:${GIT_BRANCH}
                    """
                }
            }
        }
    }
}
