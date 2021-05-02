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
		stage("build & SonarQube analysis") {
            agent any
            steps {
				scripts{
              withSonarQubeEnv('SonarQubeJenkins') {
                sh 'mvn sonar:sonar'
              timeout(time: 1, unit: 'HOURS') {
                def qgresult=waitForQualityGate()
				if (qgresult.status!= 'OK'){
					error "pipeline aborted to to quality gate failure and status is : ${qgresult.status}"
				}
				sh 'mvn clean install'
              }
            }
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
