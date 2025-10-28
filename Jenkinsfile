pipeline {
  agent any

  environment {
    IMAGE_NAME = "django-docker-demo"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
      }
    }

    stage('Test') {
      steps {
        // run Django tests inside the built image
        sh 'docker run --rm $IMAGE_NAME:latest python manage.py test'
      }
    }

    stage('Deploy with Docker Compose') {
      steps {
        // bring the stack down (if running), then up with latest build
        sh '''
          docker compose -p django_demo down || true
          docker compose -p django_demo up -d --build
        '''
      }
    }
  }

  post {
    success {
      echo '✅ Build, test, and deploy completed.'
    }
    failure {
      echo '❌ Build failed — check Console Output.'
    }
  }
}
