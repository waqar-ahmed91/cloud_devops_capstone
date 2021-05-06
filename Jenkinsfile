
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
                    source ~/.devops/bin/activate
                    ls -a
                    hadolint Dockerfile
                '''
                echo "Linting Successful"
            }
        }

        stage('Building and Pushing Docker Image') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                    sh '''#!/bin/bash
                        source ~/.devops/bin/activate
                        sudo docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        sudo docker build -t capstone:latest .
                        sudo docker tag capstone:latest 127160062615.dkr.ecr.us-west-2.amazonaws.com/capstone:latest
                        sudo docker push 127160062615.dkr.ecr.us-west-2.amazonaws.com/capstone
                        
                    '''
                    echo "Build Successful"
            } 
            }

        }
         stage('Deployment') {
              steps{
                  withAWS(credentials: 'ecr-credentials', region: 'us-west-2') {
                      sh "aws eks --region us-west-2 update-kubeconfig --name jenkins"
                      sh "kubectl config use-context arn:aws:eks:us-west-2:127160062615:cluster/jenkins"
                      sh "kubectl apply -f deployment.yml"
                      sh "kubectl get nodes"
                      sh "kubectl get deployment"
                      sh "kubectl get pod -o wide"
                      sh "kubectl get service/capstone"
                      echo "Successful Deployment"

                  }
              }
        }
        stage("Cleaning") {
              steps{
                    sh "docker system prune"
                    echo "Cleaning Complete"
              }
        }
     }
}