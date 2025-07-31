pipeline {
    agent { label 'agent-1' }

    parameters {
        string(name: 'REPO_NAME', defaultValue: 'nodejs1-kube', description: 'Nama repo untuk Docker image (misal: nodejs1-kube)')
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch untuk build (contoh: master, develop)')
        string(name: 'TAG', defaultValue: '', description: 'Tag opsional untuk build (contoh: v1.0.0), kosongkan jika tidak pakai tag')
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('gunawand') // Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    if (params.TAG?.trim()) {
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: "refs/tags/${params.TAG}"]],
                            userRemoteConfigs: [[url: 'https://github.com/gunawan-d/${params.REPO_NAME}.git']]
                        ])
                    } else {
                        checkout([
                            $class: 'GitSCM',
                            branches: [[name: "${params.BRANCH}"]],
                            userRemoteConfigs: [[url: 'https://github.com/gunawan-d/${params.REPO_NAME}.git']]
                        ])
                    }
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Tentukan tag Docker: kalau TAG diisi → pakai TAG, kalau kosong → pakai BRANCH
                    def dockerTag = params.TAG?.trim() ? params.TAG : params.BRANCH
                    def safeTag = dockerTag.replaceAll('/', '-')
                    def imageTag = "gunawand/${params.REPO_NAME}:${safeTag}"

                    sh """
                    echo "Docker login..."
                    echo "${DOCKERHUB_CREDENTIALS_PSW}" | docker login -u "${DOCKERHUB_CREDENTIALS_USR}" --password-stdin

                    echo "Building image ${imageTag}..."
                    docker build -t ${imageTag} .

                    echo "Pushing image..."
                    docker push ${imageTag}
                    """

                    sh 'docker logout'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Deploying gunawand/${REPO_NAME}:${TAG ?: BRANCH}"
                # Tambahkan perintah deploy sesuai kebutuhan (kubectl / docker-compose)
                '''
            }
        }
    }
}
