/* Requires the Docker Pipeline plugin */
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                sh 'php --version'
                git 'https://github.com/danielmordi/laravel-ci-jenkins.git'
            }
        }

        stage('Install Dependencies') {
        	steps {
        		sh 'composer install'
        		sh 'npm install'
        	}
        }

        stage('Environment Setup') {
        	steps {
				sh 'cp .env.example .env'
                sh 'php artisan key:generate'
        	}
        }

        stage('Tests') {
            steps {
                sh 'php artisan test'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    rsync -av --exclude=".git" --exclude=".env" ./ /var/www/laravel-ci-jenkins/
                    cd /var/www/laravel-ci-jenkins
                    php artisan migrate --force
                    php artisan cache:clear
                    php artisan config:clear
                '''
            }
        }
    }
}