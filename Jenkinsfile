pipeline {
    agent any
    environment {
        CONTAINER_LIST = sh(script: "docker ps --format '{{.Names}}'", returnStdout: true).trim()
    }
    parameters {
        choice(name: 'CONTAINER_NAME', choices: ['Loading...'], description: 'Select a container to update')
    }
    stages {
        stage('Prepare Choice Parameter') {
            steps {
                script {
                    def containerNames = sh(script: "docker ps --format '{{.Names}}'", returnStdout: true).trim().split("\n")
                    def containerChoices = containerNames.join("\n") // Format correctly
                    currentBuild.rawBuild.getAction(ParametersAction.class)
                        .getParameter('CONTAINER_NAME')
                        .defaultValue = containerChoices
                }
            }
        }
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
