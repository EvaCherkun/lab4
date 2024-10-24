pipeline {
    options { 
        timestamps()  
    }
    
    agent none  

    stages {
        stage('Checkout SCM') {
            agent any  
            steps {
                checkout scm  
            }
        }

        stage('Build') {
            agent any  
            steps {
                echo "Building project with build number: ${BUILD_NUMBER}"
                echo "Build completed"
            }
        }

        stage('Test') {
            agent {
                docker { 
                    image 'alpine'  
                    args '-u=\"root\"'  
                }
            }
            steps {
            sh 'apk add --update python3 py3-pip'
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip3 install Flask unittest2 && pip3 install xmlrunner'  
                sh '. venv/bin/activate && python3 test_app.py'  
                sh 'deactivate'
            }
            post {
                always {
                    junit 'test-reports/*.xml'  
                }
                success {
                    echo "Application testing successfully completed"
                }
                failure {
                    echo "Oooppss!!! Tests failed!"
                }
            }
        }
    }
}
