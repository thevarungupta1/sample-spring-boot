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
        stage('Build Jar'){
            agent {
              docker {
                image 'maven:3-alpine'
                args '-v /root/.m2:/root/.m2'
              }
            }
            steps {
                sh 'mvn clean install'
                stash includes: 'target/*.jar', name: 'spring-boot-0.0.1-SNAPSHOT'
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
