pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DPARAM_NAME_TEST=${PARAM_NAME_TEST} package'
            }
			post {
				always {
					archive ./debug
				}
			}
        }
        stage('Test') {
            steps {
                sh 'mvn  test -DPARAM_NAME_TEST=${PARAM_NAME_TEST}'
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
