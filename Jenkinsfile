pipeline {
    agent { label 'agent-1' }

    parameters {
        string(name: 'BRANCH_OR_TAG', defaultValue: 'master', description: 'Branch or Tag to build')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "${params.BRANCH_OR_TAG}"]],
                    userRemoteConfigs: [[url: 'https://github.com/gunawan-d/nodejs1-kube.git']]
                ])
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building branch/tag: ${params.BRANCH_OR_TAG}"'
                # contoh build
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying ${params.BRANCH_OR_TAG}"'
            }
        }
    }
}
