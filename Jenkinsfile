pipeline {
  agent any
  stages {
    stage('Git:S2') {
      steps {
        sh '''git clone https://github.com/httpEugene/express-service.git
'''
      }
    }
    stage('Build:S2') {
      steps {
        sh '''cd express-service
yarn install --mutex file:/var/lib/jenkins/workspace/.yarn-mutex
'''
      }
    }
    stage('Package:S2') {
      steps {
        sh '''cd express-service
docker build -t service2 .
'''
      }
    }
    stage('Deploy') {
      steps {
        parallel(
          "Service1": {
            sh '''docker run -p 3000:3000 -d service1
'''
            
          },
          "Service2": {
            sh '''docker run -p 3001:3001 -d service2
'''
            
          },
          "Service3": {
            sh '''docker run -p 3002:3002 -d service3
'''
            
          }
        )
      }
    }
    stage('Test:S2') {
      steps {
        parallel(
          "QualityCheck": {
            sh '''echo 'working on quality S2'
'''
            
          },
          "Test": {
            sh '''cd express-service
npm run test-unit
'''
            
          }
        )
      }
    }
    stage('Integration Tests') {
      steps {
        sh '''cd express-service
npm run test-integration
'''
      }
    }
    stage('Cleanup') {
      steps {
        sh 'echo \'cleaning every thing\''
      }
    }
  }
}