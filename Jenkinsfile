pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "v1.0.0" // Or pass it as parameter
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Manjesh501/gitops-register-app'
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

        stage("Commit and Push to GitHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                        git config user.name "${GIT_USERNAME}"
                        git config user.email "manjesht78@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest with image tag ${IMAGE_TAG}" || echo "No changes to commit"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Manjesh501/gitops-register-app.git HEAD:main
                    """
                }
            }
        }
    }
}
