
pipeline {
    agent any
    stages {
        stage('Installing Dependencies') {
            steps {
                sh 'sudo apt-get install python3-venv python-pip-whl python3.8-venv'
                sh 'sudo apt-get install build-essential'
                sh '''#!/bin/bash
                    python3 -m venv ~/.devops
	                  source ~/.devops/bin/activate
                    make install 
                '''
                echo "Dependencies Installation Successful"
            }
        }

        stage('Linting') {
            steps {
                sh '''#!/bin/bash
                    source ~/.devops/bin/activate
                    make lint
                '''
                ech0 "Linting Successful"
            }
        }

        stage('Building Docker Image') {
            steps {
                sh '''#!/bin/bash
                    source ~/.devops/bin/activate
                    sudo docker build --tag=capstone
                    sudo ./upload_docker.sh
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