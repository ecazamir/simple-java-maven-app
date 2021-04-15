pipeline {
    agent none
	environment {
        JAVA_VERSIONS = '8 11 17'
    }
    stages {
        stage('Matrix build and test') {
		    matrix {
			    axes {
				    axis {
					    name 'JAVA_VERSION'
						values '8', '11', '17'
					}
				}
				stages {
					agent {
						label '${JAVA_VERSION}'
						docker {
							image 'maven:3.8-openjdk-$JAVA_VERSION'
							args '-v /root/.m2:/root/.m2'
						}
					}
					stage('Build') {
						steps {
							echo "Build with Java ${JAVA_VERSION}"
							sh 'mvn --version'
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
				}
			}
        }
    }
}
