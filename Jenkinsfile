pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS=credentials('docker-hub')
    }
     
    stages {
        stage('build') {
            agent {
                docker { image 'openjdk:11-jdk' }
            }
            steps {
                sh 'chmod +x gradlew && ./gradlew build jacocoTestReport'
            }
        }
        stage('sonarqube') {
        agent {
            docker { image 'sonarqube' } }
            steps {
                sh 'echo scanning!'
            }
        }
        stage('docker build') {
            steps {
                sh 'mvn clean install'
                sh 'docker build -t varungupta2809/myapp-test:latest .'
            }
        }
        stage('docker login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('docker push') {
            steps {
               sh 'docker push varungupta2809/myapp-test:latest'
                }
            }
        stage('Deploy App') {
            steps {
                sh 'echo deploy to kubernetes'               
            }
    }
}
}
