pipeline {
    agent any

    environment {
                // AWS_ACCESS_KEY_ID = credentials('jenkins-aws-access-key-id')
                // AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
                // DOCKERHUB_USERNAME = "shivanishivani"
                APP_NAME = "clinic09"

            }

    stages {
        stage ('Updating Kubernetes deployment file') {
            steps {  
                echo 'updating app-deploy.yaml file' 
                script {
                        dir ('k8s') {
                        sh """
                        cat app-deploy.yaml
                        sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' app-deploy.yaml
                        cat app-deploy.yaml
                        """
                    }
                }
            } 
        }
        stage ('Push the changed deployment file to Git') {
            steps {  
                echo 'Pushing changed files to Git' 
                script {
                        dir ('k8s') {
                        sh """
                        git config --global user.name "Dinesh Aleti"
                        git config --global user.email "dinesh.semac9@gmail.com"
                        git add .
                        git commit -m "updated the deployment file"
                        """
                        withCredentials([gitUsernamePassword(credentialsId: 'gitlab-creds', gitToolName: 'Default')])
                            {
                           sh "git push origin HEAD:master" 
                        }
                    }
                }
            } 
        }
        // stage ('Trigger CD') {
        //     steps {  
        //         script {
        //             echo 'ArgoCD trigger of k8s manifest files'
        //             sh "kubectl apply -f application.yaml"
        //         }
        //     }
        // }
    }
}

