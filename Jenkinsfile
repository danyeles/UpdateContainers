pipeline {
    agent any
    parameters {
        activeChoice(name: 'CONTAINER_NAME', type: 'single-select', script: '''
            return sh(script: "docker ps --format '{{.Names}}'", returnStdout: true).trim().split("\\n")
        ''')
    }
    stages {
        stage('Retrieve Repository') {
            steps {
                script {
                    env.REPO_NAME = sh(
                        script: "docker ps --format '{{.Image}} {{.Names}}' | grep ${CONTAINER_NAME} | awk '{print \$1}'",
                        returnStdout: true
                    ).trim()
                    echo "Updating container ${CONTAINER_NAME} with repository ${REPO_NAME}"
                }
            }
        }
        stage('Pull Latest Image') {
            steps {
                sh "docker pull ${REPO_NAME}:latest"
            }
        }
        stage('Stop and Remove Existing Container') {
            steps {
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                """
            }
        }
        stage('Start New Container') {
            steps {
                sh """
                    docker run -d --name ${CONTAINER_NAME} ${REPO_NAME}:latest
                """
            }
        }
    }
}
