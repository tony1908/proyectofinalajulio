pipeline {
    agent any
    
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
                    docker.build("proyectofinaltesting", "--file=Dockerfile.test .")
                }
            }
        }
    }
}