pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
	
    environment {
       VALUE = readMavenPom().getProperties().getProperty('some')
    }
	
    stages {
	    stage('Prepare') {
			steps {		
				echo "value= $VALUE"
			}
		}
		
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
		
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                  junit 'target/surefire-reports/*.xml'
                }
            }
        }
		
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}