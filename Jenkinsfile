pipeline {
    agent any
    stages {
        stage("Verify tooling") {
            steps {
                sh '''
                    docker-compose exec php bash
                    docker info
                    docker version
                    docker compose version
                '''
            }
        }

        stage("Clear all running docker containers") {
            steps {
                script {
                    try {
                        sh 'docker rm -f $(docker ps -a -q)'
                    } catch(Exception e) {
                        echo ' No running container to clear up ...'
                    }
                }
            }
        }

        stage("Start Docker") {
            steps {
                sh 'make up'
                sh 'docker compose ps'
            }
        }

        stage("Run Composer Install") {
            steps {
                sh 'docker-compose exec php bash'
                sh '--rm composer install'
            }
        }

        stage("Run Tests") {
            steps {
                sh 'run artisan test'
            }
        }
    }
}
