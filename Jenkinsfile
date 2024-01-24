pipeline {
    agent any
    stages {
        stage("Verify tooling") {
            steps {
                bash '''
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
                        bash 'docker rm -f $(docker ps -a -q)'
                    } catch(Exception e) {
                        echo ' No running container to clear up ...'
                    }
                }
            }
        }
        stage("Start Docker") {
            steps {
                bash 'make up'
                bash 'docker compose ps'
            }
        }

        stage("Run Composer Install") {
            steps {
                bash 'docker-compose exec php bash'
                bash '--rm composer install'
            }
        }

        stage("Run Tests") {
            steps {
                bash 'run artisan test'
            }
        }
    }
}
