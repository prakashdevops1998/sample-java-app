pipeline {
    agent any 
    tools {
        jdk 'java17'
        maven 'Maven3'
    }
    stages { 
        stage("cleanworkspace") {
            steps {
                cleanWs()
            }
        }
        stage("checkout from SCM") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/prakashdevops1998/register-app.git']]])
            }	
        }
        stage("build application") {
            steps {
                script {
                    sh "mvn clean package"
                }
            }
        }
        stage("Test Application") {
            steps {
                script { 
                    sh "mvn test"
                }
            }
        }
        stage("SonarQube analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'Sonar-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false , credentialsId: 'Sonar-token'
                }
            }
        }
    }
}
