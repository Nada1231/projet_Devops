pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
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
        stage('Maven Package') {
            steps { dir('backend') { sh 'mvn package -DskipTests' } }
        }
    }
}
