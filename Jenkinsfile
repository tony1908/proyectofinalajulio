pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:${env.PATH}"
        IMAGE_NAME = 'proyectofinal'
        IMAGE_NAME_TEST = 'proyectofinaltesting'
        DOCKER_USERNAME = 'toony1908'
        DOCKER_CREDENTIALS = 'docker-hub-creds'
        DOCKER_IMAGE = "${DOCKER_USERNAME}/${IMAGE_NAME}"
    }
    
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/tony1908/proyectofinalajulio.git'
            }
        }

        stage ('Test'){
            steps {
                script{
                    docker.build(IMAGE_NAME_TEST, "--file=Dockerfile.test .")
                }
            }
        }

        stage ('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE, ".")
                }
            }
        }
    }
}