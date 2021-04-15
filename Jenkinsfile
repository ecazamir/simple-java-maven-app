pipeline {
    agent none
	environment {
        JAVA_VERSION = '8'
    }
    stages {
        stage('Build') {
		    agent {
				docker {
					image 'maven:3.8-openjdk-$JAVA_VERSION'
					args '-v /root/.m2:/root/.m2'
				}
			}
            steps {
			    sh 'mvn --version'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
		    agent {
				docker {
					image 'maven:3.8-openjdk-$JAVA_VERSION'
					args '-v /root/.m2:/root/.m2'
				}
			}
            steps {
			    sh 'mvn --version'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
		    agent {
				docker {
					image 'maven:3.8-openjdk-$JAVA_VERSION'
					args '-v /root/.m2:/root/.m2'
				}
			}
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
