node() {
    environment {
    registry = "poornii/shopping-cart"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
    stage ('Build Install dependencies') {
        nodejs('nodejs'){
        echo "Modules installed"
        sh "npm install"
      }
    }
    stage ('Build completed') {
        nodejs('nodejs'){
        echo "Build completed"
        sh "npm run ng -- build --prod"
      }
    }
    stage ('Build Docker Image') {
       nodejs('nodejs'){
        echo "Building Docker Image"
          script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          }
        }
    }
    stage ('Push Docker Image') {
       nodejs('nodejs'){
        echo "Pushing Docker Image"
         script {
              docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
              dockerImage.push('latest')
              }
        }
      }
    }
    stage ('Deploy to Dev') {
        echo "Deploying to Dev Environment"
         sh "docker rm -f shoppingcart || true"
         sh "docker run -d --name=shoppingcart -p 8081:8080 poornii/shopping-cart"
      }
    }
  

