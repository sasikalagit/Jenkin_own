node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "script: './mvnw --batch-mode -Dmaven.test.failure.ignore=true test'"
	}

	stage ('package'){
		sh "script: './mvnw --batch-mode package -DskipTests'"
	}
	
	stage ('Junit'){
		sh "junit(testResults: 'target/surefire-reports/*.xml', allowEmptyResults : true)"

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'Tomcatcreds', path: '',  url: 'http://ec2-54-152-29-157.compute-1.amazonaws.com:8080/')], contextpath: null, war: 'target/*.war'
	}
	
	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "isasalakr@gmail.com"
		    )
	}
}
