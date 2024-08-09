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

        stage('Test') {
            steps {
                    sh 'docker-compose run app php artisan migrate --database=mysql_testing'
                    sh 'docker-compose run app php artisan db:seed --class=DummyContentSeeder --database=mysql_testing'
                    sh 'docker-compose run app php vendor/bin/phpunit'
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
