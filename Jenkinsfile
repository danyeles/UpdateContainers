pipeline {
    agent any
    parameters {
        choice(name: 'CONTAINER_NAME', choices: [
            'lscr.io/linuxserver/lidarr',
            'ghcr.io/gethomepage/homepage',
            'meetingsdk-headless-linux-sample-zoomsdk',
            'ghcr.io/hotio/readarr',
            'ghcr.io/hotio/bazarr',
            'lscr.io/linuxserver/prowlarr',
            'ghcr.io/hotio/prowlarr',
            'linuxserver/sonarr',
            'lscr.io/linuxserver/sonarr',
            'linuxserver/radarr',
            'hello-world',
            'qbittorrentofficial/qbittorrent-nox',
            'ghcr.io/flaresolverr/flaresolverr'
        ], description: 'Select a container to update')
    }
    stages {
        stage('Pull Latest Image') {
            steps {
                sh "docker pull ${CONTAINER_NAME}:latest"
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
                    docker run -d --name ${CONTAINER_NAME} ${CONTAINER_NAME}:latest
                """
            }
        }
    }
}
