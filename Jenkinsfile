pipeline {
    agent none
    stages {
        stage('Matrix build and test') {
	    matrix {
		axes {
		    axis {
		        name 'JAVA_VERSION'
			values '8', '11'
		    }
		}
	        agent {
	            docker {
	                image 'maven:3.8-openjdk-$JAVA_VERSION'
		        args '-v /root/.m2:/root/.m2'
	            }
	        }
		stages {
		    stage('Build Java $JAVA_VERSION') {
		        steps {
		            echo "Build with Java ${JAVA_VERSION}"
		            sh 'mvn --version'
		            sh 'mvn -B -DskipTests clean package'
		        }
		    }
		    stage('Test Java $JAVA_VERSION') {
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
