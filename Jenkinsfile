pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
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
        stage('Deploy') {
            steps {
                echo 'Deploying'
		pushToCloudFoundry(
		target: 'api.run.pivotal.io',
		organization: 'Channel',
		cloudSpace: 'cdevarenne',
		credentialsId: 'cc552891-a30a-4a98-bb1f-1ea2137bad04',
		manifestChoice: [
		  appName: 'simple-java-maven-app', 
		  appPath: 'target/my-app-1.0-SNAPSHOT.jar', 
		  instances: '1', memory: '1024', 
		  value: 'jenkinsConfig'
		]
		)
            }
        }
    }
}
