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
            sh 'export PATH=$PATH:/var/lib/jenkins/.local/bin && bandit -f xml -o bandit-output.xml -r . || true'
        }
        recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
    }
}

    }
}
