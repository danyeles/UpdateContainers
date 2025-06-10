pipeline {
    agent any
    parameters {
        choice(name: 'CONTAINER_NAME', choices: [
            'lidarr',
            'homepage',
            'meetingsdk-headless-linux-sample-zoomsdk',
            'readarr',
            'bazarr',
            'prowlarr',
            'prowlarr-hotio',
            'sonarr',
            'sonarr-linuxserver',
            'radarr',
            'hello-world',
            'qbittorrent-nox',
            'flaresolverr'
        ], description: 'Select a container to update')
    }
    stages {
        stage('Retrieve Repository') {
            steps {
                script {
                    def repoMap = [
                        'lidarr': 'lscr.io/linuxserver/lidarr',
                        'homepage': 'ghcr.io/gethomepage/homepage',
                        'meetingsdk-headless-linux-sample-zoomsdk': 'meetingsdk-headless-linux-sample-zoomsdk',
                        'readarr': 'ghcr.io/hotio/readarr',
                        'bazarr': 'ghcr.io/hotio/bazarr',
                        'prowlarr': 'lscr.io/linuxserver/prowlarr',
                        'prowlarr-hotio': 'ghcr.io/hotio/prowlarr',
                        'sonarr': 'linuxserver/sonarr',
                        'sonarr-linuxserver': 'lscr.io/linuxserver/sonarr',
                        'radarr': 'linuxserver/radarr',
                        'hello-world': 'hello-world',
                        'qbittorrent-nox': 'qbittorrentofficial/qbittorrent-nox',
                        'flaresolverr': 'ghcr.io/flaresolverr/flaresolverr'
                    ]
                    env.REPO_NAME = repoMap[CONTAINER_NAME]
                    echo "Updating ${CONTAINER_NAME} with repository ${env.REPO_NAME}"
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
