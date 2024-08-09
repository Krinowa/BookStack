pipeline {
    agent any
    stages {
        step("verify tooling" {
            steps {
                sh '''
                    docker version
                    docker info
                    docker compose version
                    curl --version
                    jq --version
                '''
            }
        })
    }
}
