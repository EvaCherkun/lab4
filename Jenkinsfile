pipeline {
    options { 
        timestamps()  
    }
    environment {
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    agent {
        docker {
            image 'alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm  
            }
        }

        stage('Build') {
            steps {
                echo "Building project with build number: ${BUILD_NUMBER}"
                echo "Build completed"
            }
        }

        stage('Test') {
            agent {
                docker { 
                    image 'alpine'  
                    args '-u root -v /var/run/docker.sock:/var/run/docker.sock'  
                }
            }
            steps {
             
