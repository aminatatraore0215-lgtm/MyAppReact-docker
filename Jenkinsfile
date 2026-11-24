pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build') {
      steps { bat 'docker compose build' }
    }

    stage('Run') {
      steps { bat 'docker compose up -d' }
    }

    stage('Test') {
      steps {
        bat 'ping -n 11 127.0.0.1 > nul'
        bat 'curl -f http://localhost:9000/api/health'
        bat 'curl -f -I http://localhost:3000'
      }
    }
  }

  post {
    always { bat 'docker compose down' }
    success { echo 'Build+Test OK' }
    failure { echo 'Build/Test KO' }
  }
}
