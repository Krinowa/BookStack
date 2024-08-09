pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                // Build the application
                bat 'docker-compose build'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker-compose up'
            }
        }

        stage('Test') {
            steps {
                bat 'docker-compose run --rm app php artisan migrate --database=mysql_testing'
                bat 'docker-compose run --rm app php artisan db:seed --class=DummyContentSeeder --database=mysql_testing'
                bat 'docker-compose run --rm app php vendor/bin/phpunit'
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
