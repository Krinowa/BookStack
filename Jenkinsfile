pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('krinowa-dockerhub')
    }

    stages {

        stage('Build') {
            steps {
                // Build the application
                sh 'docker compose build'
            }
        }

        stage("Login") {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Test') {
            steps {
                // Run tests using Docker
                script {
                    docker.image('php:8.3.9-alpine3.20').inside {
                        sh 'docker-compose run app php artisan migrate --database=mysql_testing'
                        sh 'docker-compose run app php artisan db:seed --class=DummyContentSeeder --database=mysql_testing'
                        sh 'docker-compose run app php vendor/bin/phpunit'
                    }
                }
            }
        }

    }

    post {
        always {
            // Cleanup workspace or send notifications
            cleanWs()
        }
    }
}
