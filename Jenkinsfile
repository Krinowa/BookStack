pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('krinowa-dockerhub')
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Run Composer and NPM install
                sh 'composer install'
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the application
                sh 'npm run build'
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
