pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    environment {
        SONAR_HOME = tool 'sonar-scanner'
    }
 
    stages {
        stage('Github checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/surajkk93/FullStack-Blogging-App.git'
            }
        }
        stage('maven compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('maven test') {
            steps {
                sh "mvn test"
            }
        }
        stage('trivy file system scan') {
            steps {
                sh "trivy fs --format table -o fs-report.html ."
            }
        }
        stage('Sonarqube code analysis') {
            steps {
               withSonarQubeEnv('sonar-scanner') {
                    sh '''$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=Blogging-app -Dsonar.projectKey=Blogging-app \
                    -Dsonar.java.binaries=target'''
                }
            }
        }
        stage('Build ') {
            steps {
                sh "mvn package"
            }
        }
        stage('Publish artifect') {
            steps {
              withMaven(globalMavenSettingsConfig: 'maven-setting', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                  sh 'mvn deploy'
                }
            }
        }
    }
}
 
