pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:${env.PATH}"
        IMAGE_NAME = 'proyectofinal'
        IMAGE_NAME_TEST = 'proyectofinaltesting'
        DOCKER_USERNAME = 'toony1908'
        DOCKER_CREDENTIALS = 'docker-hub-credentials'
        DOCKER_IMAGE = "${DOCKER_USERNAME}/${IMAGE_NAME}"
        EMAIL_FROM = 'tony1908@gmail.com'
        EMAIL_TO = 'tony1908@gmail.com'
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
                    image = docker.build(DOCKER_IMAGE, ".")
                }
            }
        }
4
        stage ('Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        image.push()
                    }
                }
            }
        }

        stage ('Deploy') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/namespace.yaml'
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }
    }

    post {
        failure {
            mail to: EMAIL_TO, from: EMAIL_FROM, subject: "Jenkins build failed", body: "Jenkins build failed"
        }
    }
}