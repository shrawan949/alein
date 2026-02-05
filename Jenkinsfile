pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/shrawan949/alein.git'
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

        stage('Build Maven') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'Docker-Hub', toolName: 'docker') {
                        sh "docker build -t swapnilhub/loginwebappseven ."
                        sh "docker push swapnilhub/loginwebappseven:latest"
                    }
                }
            }
        }

        stage('TRIVY Scan') {
            steps {
                sh "trivy image swapnilhub/loginwebappseven:latest > trivyimage.txt"
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh "docker run -d --name=loginwebseven1 -p 8083:8080 swapnilhub/loginwebappseven:latest"
            }
        }

    } // end stages
} // end pipeline

