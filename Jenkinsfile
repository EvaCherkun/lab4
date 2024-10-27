pipeline {
    options { 
        timestamps()  
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
                    args '-u root'  
                }
            }
            steps {
                sh 'apk add --update python3 py3-pip'  // Встановлення Python та pip
                sh '. venv/bin/activate && pip3 install Flask unittest2 xmlrunner'  // Встановлення залежностей
                sh '. venv/bin/activate && python3 test_app.py'  // Запуск тестів
            }
            post {
                always {
                    junit 'test-reports/*.xml'  // Вказівка на шлях до звітів JUnit
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
