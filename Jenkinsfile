pipeline {
	agent any

stages {
	stage('Checkout') {
		steps {
                	checkout([$class: 'GitSCM',
                	branches: [[name: '*/main']],
                	userRemoteConfigs: [[
                        	url: 'https://github.com/Kyne21/sast-demo-app.git',
                        	credentialsId: 'github-token' // Menggunakan GitHub Token yang sudah ditambahkan di Jenkins Credentials
                    	]]
                	])
            	}
	}
	stage('Install Dependencies') {
		steps {
			sh 'pip install bandit'
		}
	}
	stage('SAST Analysis') {
		steps {
			sh 'bandit -f xml -o bandit-output.xml -r . || true'
			recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
		}
	}
}
}
