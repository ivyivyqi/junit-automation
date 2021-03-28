pipeline {

    agent any
    stages {

        stage('Checkout Codebase'){
            steps{
                cleanWs()
                checkout scm: [$class: 'GitSCM', branches: [[name: '*/main']],userRemoteConfigs:
                [[credentialsId: 'mygithub', url: 'https://github.com/ivyivyqi/junit-automation.git']]]
            }
        }

        stage('Build'){
            steps{
                bat 'mkdir lib'
                bat '''cd lib
                copy D:\\junit\\junit-platform-console-standalone-1.7.0-all.jar  .
                '''
                bat '''cd src
                javac -cp ../lib/junit-platform-console-standalone-1.7.0-all.jar CarTest.java Car.java ./App.java
                '''
            }
        }

        stage('Test'){
            steps{
                bat 'cd src/ ; java -jar ../lib/junit-platform-console-standalone-1.7.0-all.jar -cp "." --select-class CarTest --reports-dir="reports"'
                junit 'src/reports/*-jupiter.xml'
            }
        }

        stage('Deploy'){
            steps{
                bat 'cd src/ ; java App' 
            }
        }
    }

}
