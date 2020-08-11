pipeline {
    environment{
    registry = "kt98/shoppingcart"
    registryCredential = "docker_hub_kishan"
    dockerImage = ''
  }
  agent any
  parameters {
    gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
  }
  stages {
    stage('git') {
      steps {
        echo 'checkout code from github'
        git branch: "${params.BRANCH}", url: 'https://github.com/k-98/shopping-cart.git'
      }
    }
     stage('install dependendience') {
      steps {
          echo 'installing dependencies'
          sh 'npm install'
        
            }
    }
     stage('build the project') {
      steps {
        echo 'building the project'
        sh 'npm run build'
       
      }
    }
     stage('Build docker image'){
      steps{
        echo "Building docker image"
        script{
          dockerImage = docker.build registry + ":$BUILD_NUMBER" 
        }
      }
    }
    stage('Push docker image'){
      steps{
        echo "Pushing docker image"
        script{
           docker.withRegistry('',registryCredential) {
            dockerImage.push()
            dockerImage.push('latest')
          }
        }
      }      
    }
    stage('Deploy to Dev'){   
      steps{
        echo "Deploying to dev environment"
        sh 'docker rm -f chairshoppingcart || true'
        sh 'docker run -d --name=chairshoppingcart -p 5000:80 kt98/shoppingcart'
        //sh 'npm start'
      }
    }
    
  }
}
