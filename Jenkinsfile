pipeline {
  agent any
  stages {
    stage('Installing Dependencies') {
      steps {
        sh ''' sh \'\'\'
    #!/bin/bash
    python3 -m venv ~/.devops
    source ~/.devops/bin/activate
    make install 
    \'\'\''''
      }
    }

    stage('Linting') {
      steps {
        sh '''sh \'\'\'
    #!/bin/bash
    python3 -m venv ~/.devops
    source ~/.devops/bin/activate
    make lint
    \'\'\''''
      }
    }

    stage('Building') {
      steps {
        sh '''sh \'\'\'
    #!/bin/bash
    source ~/.devops/bin/activate
    sudo docker build --tag=flasksklearn .
    sudo ./upload_docker.sh
    \'\'\''''
        echo 'Build Successful'
      }
    }

  }
}