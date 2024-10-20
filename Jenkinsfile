pipeline {
  agent any
  stages {
    stage('CI : Checkout') {
      steps {
        git(url: 'https://github.com/Sangeetha2001/Pipelines', branch: 'main')
      }
    }

    stage('CI : Build Docker Image') {
      steps {
        sh '''
sudo docker image build -t redis:v7.2.5 -f image/redis/7.2.5/redis.dockerfile image/redis/7.2.5/context'''
      }
    }

    stage('CI : Testing') {
      steps {
        input 'App testing has been successful. Do you want to proceed deployment in staging environment?'
      }
    }

    stage('CD : Prod deployment') {
      steps {
        input 'Deployment has been successful. Do you want to proceed deployment in production environment?'
      }
    }

  }
}