pipeline {
    agent {
        label 'jenkins-agent'
    }

    environment {
        APP_NAME = "complete-production-e2e-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/AhmedSamy1999/ArgoCD-GitOps-Kubernetes'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                cat deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                cat deployment.yaml
                """
            }
        }

            stage("Push the changed deployment file to Git") {
                steps {
                    sh """
                    git config --global user.name "AhmedSamy1999"
                    git config --global user.email "ahmed.s.elsherbiny0@gmail.com"
                    git add deployment.yaml
                    git diff --cached --quiet || git commit -m "Updated Deployment Manifest"
                    """
                    
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh """
                        git diff --cached --quiet || git push https://github.com/AhmedSamy1999/complete-prodcution-e2e-pipeline main
                        """
                    }
                }
            }

    }
}
