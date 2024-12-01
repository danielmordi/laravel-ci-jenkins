/* Requires the Docker Pipeline plugin */
pipeline {
    agent any
    enviroment {

    }
    stages {
        stage('Checkout') {
            steps {
                sh 'php --version'
                git 'https://github.com/danielmordi/laravel-ci-jenkins.git'
            }
        }
        stage('Environment Setup') {
        	steps {
				sh 'cp .env.example .env'
                // Set required enviroment variables
                withCrendentials([usernamePassword(credentialsId: 'dbconfig', usernameVariable: 'DB_USERNAME', passwordVariable: 'DB_PASSWORD')]) {
                    sh 'echo "DB_CONNECTION=mysql" >> .env'
                    sh 'echo "DB_DATABASE=laravel_ci" >> .env'
                    sh 'echo "DB_USERNAME=$DB_USERNAME >> .env"'
                    sh 'echo "DB_PASSWORD=$DB_PASSWORD >> .env"'
                }
                sh 'php artisan key:generate'
                sh 'php artisan storage:link'
        	}
        }
        stage('Install Dependencies') {
        	steps {
        		sh 'composer install'
        		sh 'npm install'
        	}
        }
        stage('Tests') {
            steps {
                sh 'php artisan test'
            }
        }
        // stage('Deploy') {
        //     steps {
        //         sh '''
        //             rsync -av --exclude=".git" --exclude=".env" ./ /var/www/laravel-ci-jenkins/
        //             cd /var/www/laravel-ci-jenkins
        //             php artisan migrate --force
        //             php artisan cache:clear
        //             php artisan config:clear
        //         '''
        //     }
        // }
    }
}
