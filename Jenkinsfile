pipeline {
    agent any
    tools{
        jdk 'java-17'
        maven 'Maven3.9'
    }
     stages{
        stage("Git Checkout"){
            steps{
                git branch: 'master', changelog: false, poll: false, url: 'https://github.com/devops-catchup/LoginWebAppApplicationWith-Docker.git'
            }
        }
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
pipeline {
    agent any                // where to run the pipeline

    environment {            // global environment variables/tools
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {                 // all your stages go here

        stage('Checkout') {
            steps {
                git 'https://your-repo-url.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh """
                    $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Loginwebapp \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Loginwebapp
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean install"
            }
        }

        // Add more stages here (Docker build, TRIVY, Deploy, etc.)

    } // end of stages
} // end of pipeline

