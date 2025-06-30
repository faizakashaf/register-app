pipeline {
  agent {
    label "Jenkins-Agent"
  }

  tools {
    jdk 'Java17'
    maven 'Maven3'
  }
  environment{
    APP_NAME = 'register-app'
    RELEASE = '1.0.0'
    DOCKER_USER = 'faizakashaf12'
    DOCKER_IMAGE = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    DOCKER_TAG = 'v1.0'
    DOCKER_PASS = 'docker hub'  
  }

  stages {
    stage("Cleanup Workspace") {
      steps {
        cleanWs()
      }
    }

    stage("Checkout From SCM") {
      steps {
        git branch: 'faiza-test-branch',
            credentialsId: 'github',
            url: 'https://github.com/faizakashaf/register-app'
      }
    }

    stage("Build Application Artifacts") {
      steps {
        sh "mvn clean package"
      }
    }

    stage("Testing of Application") {
      steps {
        sh "mvn test"
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube Server') {  
          sh 'mvn sonar:sonar'
        }
      }
    }

   stage('Docker Login') {
    steps {
     withCredentials([usernamePassword(credentialsId: 'docker hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
       sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
       sh ' Docker logged in successfully'
     }
   }
  }
  stage('Docker Build and Push'){
    steps {
      sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .' 
      sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
    }
 }
}
