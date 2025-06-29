pipeline{
    agent{
        label "Jenkins-Agent"
    }
    tools{
      jdk 'Java17'
      maven "Maven3"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
            }

        stage("Checkout From SCM"){
            steps{
                git branch: 'faiza-test-branch' , credentialsId: 'github' , url: 'https://github.com/faizakashaf/register-app'
            }
            }

        stage("Build Application Artifacts"){
            steps{
                sh "mvn clean package" 
            }
            }

        stage("Testing of Application"){
            steps{
                sh "mvn test" 
            }
            }
        }
    }
    
