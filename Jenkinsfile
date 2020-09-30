pipeline{
        agent any
        environment {
            app_version = 'v1'
            rollback = 'True'
        }
        stages{
            stage('Build Image'){
                steps{
                    script{
                        if (env.rollback == 'false'){
                            image = docker.build("dockeryashnw/chaperoo-frontend")
                        }
                    }
                }
            }
            stage('Tag & Push Image'){
                steps{
                    script{
                        if (env.rollback == 'false'){
                            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials'){
                                image.push("${env.app_version}")
                            }
                        }
                    }
                }
            }
            stage('Deploy App'){
                steps{
                    sh "docker-compose pull && docker-compose up -d"
                }
            }
            stage('Run file'){
                steps{
                    sh "chmod +x run.sh"
                      sh "./run.sh"
                }
            }
        }
}
