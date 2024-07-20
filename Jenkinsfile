pipeline {
    agent {
		docker {
			image 'composer:latest'
		}
	}
    environment {
        NVD_API_KEY = credentials('NVD_API_KEY')
        SONARQUBE_URL = 'http://172.10.10.4:9000'
        SONARQUBE_SCANNER = tool name: 'sonar-scanner'
        SONARQUBE_TOKEN = credentials('SONARQUBE_TOKEN')
        SONARQUBE_PROJECT_KEY = 'SSDPractice'
    }
    stages {       
        stage('OWASP DependencyCheck') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'owasp', nvdCredentialsId: 'NVD_API_KEY'
            }
        }
        // stage('SonarQube Analysis') {
        //     steps {
        //         script {
        //             withSonarQubeEnv('SonarQube') {
        //                 sh '''
        //                 ${SONARQUBE_SCANNER}/bin/sonar-scanner \
        //                 -Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} \
        //                 -Dsonar.sources=. \
        //                 -Dsonar.host.url=${SONARQUBE_URL} \
        //                 -Dsonar.token=${SONARQUBE_TOKEN} \
        //                 -Dsonar.python.version=3.9
        //                 '''
        //             }
        //         }
        //     }
        // }
    }

    

    post {
        always {
            junit testResults: 'logs/unitreport.xml'
        }
        // success {
            // archiveArtifacts artifacts: 'dependency-check-report.xml, dependency-check-report.html', allowEmptyArchive: true
            // dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            // cleanWs(deleteDirs: true, disableDeferredWipeout: true)
        // }
    }
}
