
pipeline {
    agent any
    stages {
        stage('Installing Dependencies') {
            steps {
                sh '''#!/bin/bash
                    python3 -m venv ~/.devops
	                  source ~/.devops/bin/activate
                    make install 
                '''
                echo "Dependencies Installation Successful"
            }
        }

        stage('Linting Docker File') {
            steps {
                sh '''#!/bin/bash
                    ls -a
                    hadolint Dockerfile
                '''
                echo "Linting Successful"
            }
        }

        stage('Building Docker Image') {
            steps {
                sh '''#!/bin/bash
                    source ~/.devops/bin/activate
                    sudo docker build --tag=capstone
                    ls -a
                    sudo upload_docker.sh
                '''
                echo "Building Successful"
            }
        }
         stage('Deployment') {
              steps{
                  withAWS(credentials: 'ecr-credentials', region: 'us-west-2') {
                      sh "aws eks --region us-west-2 update-kubeconfig --name jenkins"
                      sh "kubectl config use-context arn:aws:eks:us-west-2:127160062615:cluster/jenkins"
                      sh "kubectl apply -f deployment.yaml"
                      echo "Successful Deployment"
                  }
              }
        }
        stage("Cleaning") {
              steps{
                    sh "docker system prune -f"
                    echo "Cleaning Complete"
              }
        }
     }
}