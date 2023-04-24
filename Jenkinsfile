pipeline {
    agent any
    tools{
        maven 'maven3.8'
    }
    stages{
        stage('Clone repo'){
            steps{
                checkout scm: scmGit(branches: [[name: '*/develop']], extensions: [], user)
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Dependency checker'){
            steps{
                dependencyCheckPublisher pattern: 'target/dependency-check.xml'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('sonarqube'){
                    sh 'mvn clean verify sonar:sonar -Dmaven.test.skip=true -Dmaven.test.failure.ignore=true -Dsonar.projectName=webgoat -Dsonar.projectKey=webgoat -Dsonar.propjectVersion=1.0 -Dsonar.exclusions=**/*.ts -Dmaven.login=<token>'
                }
            }
        }
    }
}
