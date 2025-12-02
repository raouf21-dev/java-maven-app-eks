def gv

pipeline {   
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    gv.buildJar()

                }
            }
        }

        stage("build image") {
            steps {
                script {
                    gv.buildImage()
                }
            }
        }

        stage("deploy") {
    environment {
        AWS_ACCESS_KEY_ID = credentials('jenkins_aws_access_key_id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins_aws_secret_access_key')
        AWS_DEFAULT_REGION = 'eu-west-3'
    }
    steps {
        script {
            echo 'deploying the application...'
            
            sh '''
                # Check which IAM identity is being used
                aws sts get-caller-identity
                
                # Update kubeconfig
                aws eks update-kubeconfig --name demo-cluster --region eu-west-3
                
                # Try kubectl
                kubectl get nodes
            '''
        }
    }
}            
    }
} 
