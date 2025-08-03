pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "v1.0.0" // Can be parameterized
        GIT_REPO = "https://github.com/Manjesh501/gitops-register-app"
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
    }
}
