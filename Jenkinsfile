pipeline {
  agent any
  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/IT22049490/SkillFlow-Paf.git'
      }
    }

    stage('Build and Test') {
      agent {
        docker { image 'maven:3.9.6-eclipse-temurin-17' }
      }
      steps {
        sh 'cd PafBackend && mvn clean package -DskipTests'
      }
    }

    stage('Static Code Analysis') {
      agent {
        docker { image 'maven:3.9.6-eclipse-temurin-17' }
      }
      environment {
        SONAR_URL = "http://20.245.205.56:9000"
      }
      steps {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
          sh 'cd PafBackend && mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        }
      }
    }

    stage('Build and Push Docker Image') {
      steps {
        script {
          // Build Docker image using backend Dockerfile
          def dockerImage = docker.build("fasnas/ultimate-cicd:${BUILD_NUMBER}", "PafBackend")
          
          // Push to Docker Hub
          docker.withRegistry('https://index.docker.io/v1/', 'docker-cred') {
            dockerImage.push()
          }
        }
      }
    }

    stage('Update Deployment File') {
      steps {
        withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
          sh '''
            git config user.email "mohamedfasnas999@gmail.com"
            git config user.name "Mohamed Fasnas"
            sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" PafBackend/Deployment.yml
            git add PafBackend/deployment.yml
            git commit -m "Update deployment image to version ${BUILD_NUMBER}"
            git push https://${GITHUB_TOKEN}@github.com/IT22049490/SkillFlow-Paf HEAD:main
          '''
        }
      }
    }

  }
}
