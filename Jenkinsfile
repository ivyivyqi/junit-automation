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
                bat '''cd src
                java -jar ../lib/junit-platform-console-standalone-1.7.0-all.jar -cp "." --select-class CarTest --reports-dir="reports"
                '''
                junit 'src/reports/*-jupiter.xml'
            }
        }

        stage('Deploy'){
            steps{
                bat '''cd src 
                java App
                '''
            }
        }
        
        post {  
         always {  
             echo 'This will always run'  
         }  
         success {  
              mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "bqi@methodist.edu";  
         }  
         failure {  
             mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "bqi@methodist.edu";  
         }  
         unstable {  
             echo 'This will run only if the run was marked as unstable'  
         }  
         changed {  
             echo 'This will run only if the state of the Pipeline has changed'  
             echo 'For example, if the Pipeline was previously failing but is now successful'  
         }  
     }  

}
