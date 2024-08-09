pipeline {
    agent any
    stages {
        stage("verify tooling") {
            steps {
                bat '''
                    docker version
                    docker info
                    docker compose version
                    curl --version
                    jq --version
                '''
            }
        }
    }
}
