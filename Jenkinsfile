pipeline {
  agent {
    docker {
      image 'node:10.13.0'
    }
  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        // 1.	Enable Veracode Interactive for the steps that run the tests. 
        wrap([$class: 'VeracodeInteractiveBuildWrapper', location: 'host.docker.internal', port: '10010']) {
          // 2.	Download the IAST Agent into the project workspace. 
          sh 'curl -sSL https://s3.us-east-2.amazonaws.com/app.veracode-iast.io/iast-ci.sh |  sh'
          // 3.	Run the tests with the Veracode Interactive Agent attached. 
          sh 'LD_LIBRARY_PATH=$PWD NODE_ENV=test npm run test-server'
        }
      }
    }
    stage('Deploy') {
      steps {
        sh 'echo npm package would run here...'
      }
    }
  }
}