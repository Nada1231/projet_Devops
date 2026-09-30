pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
        IMAGE_NAME = 'salmakacem29/kacemsalma-5arctic11-gestionprojets'
    }

    stages {
        stage('Checkout Git') {
            steps {
                git branch: 'salma_branche',
                    url: 'https://github.com/Nada1231/projet_Devops.git',
                    credentialsId: 'github-cred'
            }
        }
        stage('Maven Clean') {
            steps { dir('backend') { sh 'mvn clean' } }
        }
        stage('Maven Compile') {
            steps { dir('backend') { sh 'mvn compile' } }
        }
        stage('Maven Test') {
            steps { dir('backend') { sh 'mvn test' } }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    dir('backend') {
                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar -Dsonar.projectKey=projet-devops-salma -Dsonar.host.url=http://localhost:9000 -Dsonar.login=$SONAR_TOKEN'
                    }
                }
            }
        }
        stage('Maven Package') {
            steps { dir('backend') { sh 'mvn package -DskipTests' } }
        }
        stage('Docker Build') {
            steps {
                dir('backend') {
                    sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest ."
                }
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Deploy (Docker Compose)') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d'
            }
        }
    }
    post {
        always {
            sh 'docker logout || true'
        }
    }
}
