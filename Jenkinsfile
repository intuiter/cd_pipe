pipeline {
    agent any

    environment {
                APP_NAME = "818104450483.dkr.ecr.us-east-1.amazonaws.com//pc-ecr"
            }

    stages {
        stage ('Updating Kubernetes deployment file2') {
            steps {  
                echo 'updating app-deploy.yaml file' 
                script {
                        dir ('k8s') {
                        sh """
                        cat app-deploy.yaml
                        sed -i 's/pc-ecr.*/pc-ecr:${IMAGE_TAG}/g' app-deploy.yaml  
                        cat app-deploy.yaml
                        """
                    }
                }
            } 
        }
        
        
        stage ('Push the changed deployment yaml file to Git') {
            environment {
            GIT_REPO_NAME = "cd_pipeline"
            GIT_USER_NAME = "intuiter"
            }
            steps {  
                echo 'Pushing changed files to Git' 
                script {
                        dir ('k8s') {
                        sh """
                        touch samplefile.txt
                        git config --global user.name "intuiter"
                        git config --global user.email "medha.reddy.1044@gmail.com"
                        git add .
                        git commit -m "updated the deployment file"
                        """
                        withCredentials([string(credentialsId: 'github-cred', variable: 'GIHUB_TOKEN')]) {
                        sh 'git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:master' 
                        }
                    }
                }
            } 
        }
    }
}

