echo "Hello-pipelines"
pipeline {
	agent any
	stages {
		stage("build"){
		sh "ls"
		sh "chmod u+x ./mvnw"
			sh "./mvnw package"
		sh "ls"
		}
		parallel testsA: {
			sh "echo test A"
			sleep 3
			sleep 2
		},
		testsB:{
			sh "echo test B"
			sleep 4
		}, failFast: true
		stage("Archive"){
			archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
		}
		stage("JUnit"){
			junit '**/target/surefire-reports/TEST*.xml'
		}
		stage("JaCoCo"){
			jacoco()
		}
	}
	post{
		success{
			emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", 
			recipientProviders: [developers()], 
			subject: 'TEstingMailConfig', 
			to: 'mkaram@kn-it.com'
		}
	}
}