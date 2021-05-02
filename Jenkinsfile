pipeline {
    agent any
    tools {
    maven 'Maven3'
  }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'f982a97f-4c9c-4f01-abf2-f5befc5d305d', url: 'https://github.com/DevOpsMallesh/HCL_SampleApplication.git'
            }
        }
		stage("SonarQube analysis") {
            agent any
            steps {
				scripts{
              withSonarQubeEnv('SonarQubeJenkins') {
                sh 'mvn sonar:sonar'
				}
				}
				}
		stage("stage("Quality Gate and build") {
			steps{
              timeout(time: 1, unit: 'HOURS') {
              def qg=waitForQualityGate()
				if (qg.status!= 'OK'){
					error "pipeline aborted to to quality gate failure and status is : ${qg.status}"
				}
				sh 'mvn clean install'
              }
            }
          }
		  
        stage('CleanWS after Build'){
            steps{
                cleanWs cleanWhenSuccess: false
            }
        }
        
    }
}
