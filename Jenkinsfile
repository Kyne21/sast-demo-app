pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[
                            url: 'https://github.com/Kyne21/sast-demo-app.git',
                            credentialsId: 'github-token'
                        ]]
                    ])
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'pip install bandit'
                }
            }
        }

        stage('SAST Analysis') {
            steps {
                script {
                    // Set PATH and run Bandit
                    sh 'export PATH=$PATH:/var/lib/jenkins/.local/bin && bandit -f xml -o bandit-output.xml -r . || true'
                    
                    // Check if bandit-output.xml exists and is not empty
                    if (fileExists('bandit-output.xml') && sh(script: 'test -s bandit-output.xml', returnStatus: true) == 0) {
                        recordIssues tools: [bandit(pattern: 'bandit-output.xml')], ignoreQualityGate: true
                    } else {
                        echo 'No Bandit output or file is empty, skipping issue recording.'
                    }
                }
            }
        }
    }
}
