pipeline {
    agent any

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
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.6:sonar'
    }
  }
        }
        
}
    }
