pipeline {
    options { timestamps() }
    agent any
    stages {
        stage('Check scm') {
            steps {
                checkout scm
            }
        } // stage Check scm
        
        stage('Build') {
            steps {
                echo "Building ...${BUILD_NUMBER}"
                echo "Build completed"
            }
        } // stage Build
        
        stage('Test') {
            steps {
                script {
                    // Запустіть Docker контейнер тут
                    sh 'docker run --rm -u root alpine /bin/sh -c "apk add --update python3 py-pip && pip install Flask xmlrunner && python3 app_tests.py"'
                }
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
        } // stage Test
    } // stages
} // pipeline

