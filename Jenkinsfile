pipeline {
  agent {
    docker {
      image 'abhishekf5/maven-abhishek-docker-agent:v1'
      args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  environment {
    BACKEND_IMAGE = "fasnas/pafbackend:${BUILD_NUMBER}"
    FRONTEND_IMAGE = "fasnas/paffrontend:${BUILD_NUMBER}"
    REGISTRY_CREDENTIALS = credentials('docker-cred')
    SONAR_URL = "http://your-sonarqube-server:9000" // Optional: replace with real one if using
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/IT22049490/SkillFlow-Paf.git'
      }
    }

    stage('Build Backend') {
      steps {
        sh 'cd PafBackend && mvn clean package -DskipTests'
      }
    }

    stage('Build Frontend') {
      steps {
        sh '''
          cd Frontend
          npm install
          npm run build
        '''
      }
    }

    stage('Build & Push Docker Images') {
      steps {
        script {
          sh '''
            cd PafBackend
            docker build -t ${BACKEND_IMAGE} .
            cd ../Frontend
            docker build -t ${FRONTEND_IMAGE} .
          '''
          docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIALS) {
            docker.image("${BACKEND_IMAGE}").push()
            docker.image("${FRONTEND_IMAGE}").push()
          }
        }
      }
    }

    stage('Update Deployment YAML') {
      environment {
        GIT_REPO_NAME = "SkillFlow-Paf"
        GIT_USER_NAME = "IT22049490"
      }
      steps {
        withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
          sh '''
            git config user.email "mohamedfasnas999@gmail.com"
            git config user.name "Mohamed Fasnas"

            sed -i "s|fasnas/pafbackend:.*|fasnas/pafbackend:${BUILD_NUMBER}|g" k8s/backend-deployment.yml
            sed -i "s|fasnas/paffrontend:.*|fasnas/paffrontend:${BUILD_NUMBER}|g" k8s/frontend-deployment.yml

            git add k8s/*.yml
            git commit -m "Update image tags to ${BUILD_NUMBER}"
            git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME}.git HEAD:main
          '''
        }
      }
    }
  }
}
