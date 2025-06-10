pipeline { 
	agent any 
	
	environment {		
        BRIDGE_POLARIS_SERVERURL = 'https://polaris.blackduck.com/'
        BRIDGE_POLARIS_ACCESSTOKEN = '1ta9m547gl5k34ba7utiq0i2l29gbfpfbnn4b1dctval8nr24hbe1v17munk69qchrugp25o120d2'
        BRIDGE_POLARIS_APPLICATION_NAME = 'Black Duck - Security Demo'
        BRIDGE_POLARIS_PROJECT_NAME = 'Java-InsecureBank-Jenkins'
        BRIDGE_POLARIS_BRANCH_NAME = 'Jenkins-pipeline'
		BRIDGECLI_LINUX64 = 'https://repo.blackduck.com/bds-integrations-release/com/blackduck/integration/bridge/binaries/bridge-cli-bundle/latest/bridge-cli-bundle-linux64.zip'
	} 
	
	stages { 
        stage('Build') {
            steps {
                sh 'mvn -B compile'
            }
        }
		
        stage('Polaris Security Scan') {
            steps {
                script {
                    status = bat returnStatus: true, script: '''
                        C:/Demo/GitLab-demo/synopsys-bridge/synopsys-bridge.exe --stage polaris polaris.assessment.types=SAST,SCA polaris.reports.sarif.create=false
                    '''
                    if (status == 8) { unstable 'policy violation' }
                    else if (status != 0) { error 'bridge failure' }
                }
            }
        }
    }

    post {
		success {
			echo 'Pipeline completed successfully.'
		} 
		failure {
		echo 'Pipeline failed. Check the Poalris platform for errors.'
		}
	}
}