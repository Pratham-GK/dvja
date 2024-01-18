pipeline {
    agent any
    tools{
        maven 'maven'
    }

    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/Pratham-GK/dvja.git'
            }
        }
        stage('DP Check') {
           steps {
        dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
--prettyPrint''', odcInstallation: 'owasp-dependency-checker'
        
        dependencyCheckPublisher pattern: 'dependency-check-report.xml'
      }
        }

        stage('SonarQube analysis') {
            steps{
    withSonarQubeEnv(installationName: 'SonarQube') { // You can override the credential to be used
      sh 'mvn clean install sonar:sonar'
    }
  }
        }
        stage("Quality Gate"){
            steps{
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline :true
                }
            }
        }
        
}
    }
