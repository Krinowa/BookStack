pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'krinowa-dockerhub'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                git url: 'https://github.com/your-repository-url.git', branch: 'main'
            }
        }

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

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        sh 'docker-compose up --build -d'
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
