pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment {
         SNAP_REPO = 'vprofile-snapshot'
         NEXUS_USER = 'admin'
         NEXUS_PASS = 'admin'
         RELEASE_REPO = 'vprofile-release'
         CENTRAL_REPO = 'vpro-maven-central'
         NEXUSIP = '172.31.29.5'
         NEXUSPORT = '8081'
         NEXUS_GRP_REPO = 'vpro-maven-group'
         NEXUS_LOGIN = 'nexuslogin' 
         SONARSERVER = 'sonarserver'
         SONARSCANNER = 'sonarscanner'
    }

    stages {
        stage('Build') {
            steps {
               sh 'mvn clean install -U -DskipTests -Dmaven.repo.local=~/.m2/repository'
               }
               post {
                success {
                    echo 'Now Archiving...'
<<<<<<< HEAD
                    archiveArtifacts artifacts: '**/target/*.war'
=======
                    archiveArtifacts artifacts: '**/target/.war'
>>>>>>> 541a7010e4c5208c005c4dcbae48b693e629bdb2
                }
            }
           }
           stage('UNIT TEST'){
            steps {
                sh 'mvn clean install -U -DskipTests -Dmaven.repo.local=~/.m2/repository test'
            }
        }  
        stage ('Checkstyle Analysis'){
            steps {
                sh 'mvn clean install -U -DskipTests -Dmaven.repo.local=~/.m2/repository checkstyle:checkstyle'
            }
        } 
        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool "${SONARSCANNER}"
          }
                    steps {
            withSonarQubeEnv("${SONARSERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
          }
    }
        stage ('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true 
            }
            }
        } 
        stage ('uploadArtifact') {
            steps {
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                groupId: 'QA',
                version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                repository: "${RELEASE_REPO}",
                credentialsId: "${NEXUS_LOGIN}",
                artifacts: [
                    [artifactId: 'vproapp',
                    classifier: '',
                    file: 'target/vprofile-v2.war',
                    type: 'war']
                ]
                )
            }
        }

      }

<<<<<<< HEAD
      post{
	  always {
	 	    echo 'slack Notifications.'
		    slackSend channel: '#cicd',
			color:COLOR_MAP[currentBuild.currentResult],
			message: "*${currentBuild.currentResult}:*Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at : ${env.BUILD_URL}"
}
}
}


=======
      
}
>>>>>>> 541a7010e4c5208c005c4dcbae48b693e629bdb2
