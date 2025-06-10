def getContainerList() {
    def containers = sh(
        script: "docker ps --format '{{.Image}} {{.Names}}'",
        returnStdout: true
    ).trim().split("\n")
    
    return containers.collect { it.split(" ")[1] }.join("\n") // Extract container names
}

def getRepository(containerName) {
    def repo = sh(
        script: "docker ps --format '{{.Image}} {{.Names}}' | grep ${containerName} | awk '{print $1}'",
        returnStdout: true
    ).trim()
    return repo
}

properties([
    parameters([
        choice(name: 'CONTAINER_NAME', choices: getContainerList(), description: 'Select a container to update')
    ])
])

pipeline {
    agent any
    stages {
        stage('Retrieve Repository') {
            steps {
                script {
                    env.REPO_NAME = getRepository(CONTAINER_NAME)
                    echo "Updating container ${CONTAINER_NAME} with repository ${REPO_NAME}"
                }
            }
        }
        stage('Pull Latest Image') {
            steps {
                script {
                    sh "docker pull ${REPO_NAME}:latest"
                }
            }
        }
        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    sh """
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                    """
                }
            }
        }
        stage('Start New Container') {
            steps {
                script {
                    sh """
                        docker run -d --name ${CONTAINER_NAME} ${REPO_NAME}:latest
                    """
                }
            }
        }
    }
}
